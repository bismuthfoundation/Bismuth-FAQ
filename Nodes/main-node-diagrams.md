# Bismuth Main Node Diagrams

This document complements the written architecture notes with visual diagrams of the main Bismuth node, its protocol flows, and its ledger/storage topology.

## 1. Main Node Architecture

```mermaid
flowchart LR
    subgraph Ext["External Actors"]
        Peers["Peer Nodes"]
        Miners["Miners / Pools"]
        Wallets["Wallets / CLI Tools"]
        Rpc["JSON-RPC / Third-Party Tools"]
        PluginsExt["Plugins"]
    end

    subgraph Proc["Bismuth Node Process"]
        Main["Main Entrypoint
node.py"]
        State["Shared Runtime State
libs/node.py::Node"]
        Config["Config Loader
options.py"]
        Log["Logging
log.py + libs/logger.py"]
        Keys["Wallet / Keys
wallet_keys.py + libs/keys.py"]
        Server["Inbound TCP Server
ThreadedTCPServer"]
        Handler["Inbound Request Handler
ThreadedTCPRequestHandler"]
        ConnMgr["Connection Manager Thread
connectionmanager.py"]
        Worker["Outbound Peer Workers
worker.py"]
        PeersMgr["Peer Manager
peershandler.py"]
        Api["API Handler
apihandler.py"]
        Digest["Block Processor
digest.py"]
        Mempool["Mempool
mempool.py"]
        DB["Database Handler
dbhandler.py"]
        Consensus["Peer Height Consensus
consensus_most_common / consensus_max"]
        Plugins["Plugin Manager
plugins.py"]
        Pow["PoW Verification
mining_heavy3.py"]
        Helpers["Indexes / Helpers
aliases.py tokensv2.py essentials.py difficulty.py"]
    end

    subgraph Store["Storage"]
        Ledger["ledger.db
Full Ledger"]
        Hyper["hyper.db
Hyperblock Store"]
        Index["index.db
Aliases / Tokens"]
        MemDB["mempool.db or RAM mempool"]
        RamLedger["RAM Ledger Clone
optional"]
        PeerFiles["Peer Files
peers.txt suggested_peers.txt"]
        HeavyFile["heavy3a.bin"]
    end

    Peers --> Server
    Miners --> Server
    Wallets --> Server
    Rpc --> Server

    Main --> Config
    Main --> Log
    Main --> Keys
    Main --> Plugins
    Main --> PeersMgr
    Main --> Api
    Main --> Mempool
    Main --> DB
    Main --> Server
    Main --> ConnMgr
    Main --> State

    Server --> Handler
    Handler --> State
    Handler --> PeersMgr
    Handler --> Api
    Handler --> Digest
    Handler --> Mempool
    Handler --> DB
    Handler --> Plugins

    ConnMgr --> Worker
    ConnMgr --> PeersMgr
    ConnMgr --> Mempool
    ConnMgr --> Plugins

    Worker --> Peers
    Worker --> PeersMgr
    Worker --> DB
    Worker --> Digest
    Worker --> Mempool

    PeersMgr --> Consensus
    PeersMgr --> PeerFiles

    Digest --> Pow
    Digest --> DB
    Digest --> Mempool
    Digest --> Helpers
    Digest --> Plugins

    Api --> DB
    Api --> Mempool

    DB --> Ledger
    DB --> Hyper
    DB --> Index
    DB --> RamLedger

    Mempool --> MemDB
    Pow --> HeavyFile

    PluginsExt --> Plugins
```

## 2. Protocol Architecture

```mermaid
flowchart TB
    Remote["Remote Peer / Miner / Client"] --> Proto["Socket Protocol
connections.py
10-byte length header + JSON payload"]
    Proto --> Inbound["Inbound Command Dispatcher
node.py"]

    Inbound --> Handshake["Handshake / Peer Session
version
getversion
hello"]
    Inbound --> Sync["Chain Sync
sync
blockheight
blocksfnd
blockscf
nonewblk
blocknf
blocknfhb"]
    Inbound --> Blocks["Block Submission
block"]
    Inbound --> MempoolProto["Mempool Exchange
mempool
mpinsert
mpget
mpgetjson
mpclear"]
    Inbound --> Queries["Ledger Queries
blocklast
blockget
balanceget
diffget
statusjson
tokensget
aliasesget"]
    Inbound --> API["API Surface
api_* commands"]
    Inbound --> Control["Operational Commands
peersget
portget
addpeers
stop"]
    Inbound --> PluginCmd["Plugin Command Prefixes
extra_commands"]

    Handshake --> PeerMgr["Peers Manager"]
    Sync --> PeerMgr
    Sync --> Worker["Outbound Workers"]
    Sync --> Consensus["Peer Height Consensus"]
    Blocks --> Digest["Block Digestion / Validation"]
    MempoolProto --> Mempool["Mempool"]
    Queries --> DB["Database Handler"]
    API --> ApiHandler["ApiHandler"]
    PluginCmd --> PluginMgr["Plugin Manager"]

    Worker --> Proto
    ApiHandler --> DB
    ApiHandler --> Mempool
    Digest --> DB
    Digest --> Mempool
    Digest --> Consensus
```

## 3. Peer Sync Sequence

```mermaid
sequenceDiagram
    participant A as Local Node Worker
    participant B as Remote Peer
    participant P as Peers Manager
    participant D as Digest / DB

    A->>B: version
    A->>B: local protocol version
    B-->>A: ok
    A->>B: getversion
    B-->>A: remote version
    A->>B: hello
    B-->>A: peers
    B-->>A: peer list
    A->>P: peersync(peer list)
    B-->>A: sync
    A->>B: blockheight
    A->>B: local height
    B-->>A: remote height

    alt Remote is behind
        B-->>A: remote tip hash
        A->>B: blocksfnd or nonewblk
        B-->>A: blockscf or blocksrj
        A->>B: blocks
    else Remote is ahead
        A->>B: local tip hash
        B-->>A: blocksfnd
        A->>B: blockscf
        B-->>A: block batch
        A->>D: digest_block(block batch)
    else Fork or missing common block
        B-->>A: blocknf / blocknfhb
        A->>D: rollback path
    end

    A->>B: sendsync / sync
```

## 4. Consensus and Block Acceptance

```mermaid
flowchart TD
    Start["Peer Reports Height"] --> Add["Peers.consensus_add()"]
    Add --> Opinions["peer_opinion_dict"]
    Opinions --> Common["consensus_most_common"]
    Opinions --> Max["consensus_max"]
    Opinions --> Percent["consensus_percentage"]

    Common --> Decision["Sync Decision"]
    Max --> Decision
    Percent --> Decision

    Decision --> Old["If local chain looks stale:
prefer most common height"]
    Decision --> Fresh["If local chain looks current:
prefer longest chain"]
    Decision --> Outlier["If peer deviates too far:
warn or ban"]

    Fresh --> Candidate["Candidate Block / Block Batch"]
    Old --> Candidate
    Candidate --> Lock["Acquire node.db_lock"]
    Lock --> Validate["digest.py validation"]
    Validate --> Sig["Transaction Signatures"]
    Validate --> Age["Timestamp / Age Rules"]
    Validate --> Dup["Duplicate Signature Checks"]
    Validate --> Bal["Balance / Fees / Reward Checks"]
    Validate --> Pow["Heavy3 PoW Verification"]
    Validate --> Fork["Fork Reward Rules"]
    Validate --> Commit["Write to Active Chain DB"]
    Commit --> Persist["Copy to ledger.db
and hyper.db when needed"]
    Persist --> Update["Update last_block / hdd_block / checkpoints"]
```

## 5. Ledger and Storage Topology

```mermaid
flowchart LR
    subgraph Runtime["Runtime Data Plane"]
        Inbound["Inbound Handler Thread"]
        Outbound["Outbound Worker Thread"]
        Digest["digest.py"]
        API["apihandler.py"]
        MP["mempool.py"]
        DBH["DbHandler per thread"]
    end

    subgraph Active["Active Chain View"]
        ActiveConn["DbHandler.conn / c
Active chain connection"]
        Ram["RAM Ledger Clone
used when ram=True"]
        Hyper["hyper.db
used directly when ram=False"]
    end

    subgraph Durable["Durable Storage"]
        Ledger["ledger.db
Full ledger history"]
        Index["index.db
aliases + tokens tables"]
        MemStore["mempool.db
or in-memory mempool"]
        PeersDisk["peers.txt / suggested_peers.txt"]
    end

    subgraph Derived["Derived / Auxiliary Data"]
        Misc["misc table
difficulty per block"]
        AliasIdx["Alias Index"]
        TokenIdx["Token Index"]
        Keys["wallet.der / privkey.der / pubkey.der"]
        Mandatory["mandatory_message.json"]
    end

    Inbound --> DBH
    Outbound --> DBH
    API --> DBH
    Digest --> DBH
    MP --> MemStore

    DBH --> ActiveConn
    ActiveConn --> Ram
    ActiveConn --> Hyper

    Digest --> ActiveConn
    ActiveConn --> Misc

    ActiveConn --> Ledger
    ActiveConn --> Hyper
    DBH --> Index
    Index --> AliasIdx
    Index --> TokenIdx

    Inbound --> PeersDisk
    Outbound --> PeersDisk

    API --> Keys
    MP --> Mandatory
```

## 6. External Interface Map

```mermaid
flowchart LR
    Wallet["Wallet / Local CLI"] --> Cmd["Node Command Surface"]
    Rpc["RPC Adapter / Explorer / External Tool"] --> Cmd
    Miner["Miner / Pool"] --> Cmd
    Peer["Peer Node"] --> Cmd

    Cmd --> Status["Status / Diagnostics
statusget
statusjson
diffget
portget"]
    Cmd --> Chain["Chain Data
blocklast
blockget
block_height_from_hash"]
    Cmd --> Addr["Address / Balance
balanceget
balancegethyper
addvalidate
pubkeyget"]
    Cmd --> Indexes["Aliases / Tokens
aliasget
aliasesget
addfromalias
tokensget"]
    Cmd --> TX["Transactions / Mempool
mpinsert
mpget
api_mempool"]
    Cmd --> Sync["Peer Sync / Replication
version
hello
sync
blockheight
blocksfnd"]
    Cmd --> Control["Node Control
peersget
addpeers
stop"]
    Cmd --> API["Extended API
api_*"]
```

## 7. Reading Guide

Use the diagrams together:

- Diagram 1 shows the complete runtime component map.
- Diagrams 2 and 3 show how peers and clients talk to the node.
- Diagram 4 shows how peer opinions become block acceptance decisions.
- Diagram 5 shows where chain, index, and mempool data live.
- Diagram 6 shows the external command surface by functional area.
