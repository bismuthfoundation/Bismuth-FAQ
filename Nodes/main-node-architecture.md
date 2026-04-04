# Bismuth Main Node Architecture

This document explains how the main Bismuth node works, how the major modules are connected, and how data moves through the process at runtime.

The main entrypoint is `node.py`. It starts the node, wires the core modules together, opens the inbound TCP server, starts the outbound connection manager, and then stays alive until shutdown.

## 1. High-Level Structure

The node is a single Python process with a shared `Node` state object and several cooperating modules:

| Module | Responsibility |
| --- | --- |
| `node.py` | Process startup, network mode selection, inbound request server, command dispatch, shutdown loop |
| `libs/node.py` | Shared runtime state container used across modules |
| `options.py` | Loads `config.txt` and optional `config_custom.txt` into typed config values |
| `peershandler.py` | Peer list management, connection pool tracking, bans, consensus view, peer discovery |
| `connectionmanager.py` | Long-lived manager thread that periodically launches outbound peer workers |
| `worker.py` | Outbound peer session logic and chain sync client behavior |
| `connections.py` | Socket protocol helpers: fixed-length header plus JSON payload |
| `dbhandler.py` | Per-thread SQLite connections for ledger, hyperblock, RAM, and index databases |
| `digest.py` | Block validation, PoW verification, balance checks, and block commit flow |
| `mempool.py` | Pending transaction storage, validation entrypoint, purge, relay decisions |
| `apihandler.py` | Extra `api_*` commands for external tools and RPC adapters |
| `plugins.py` | Plugin loading and hook execution |

At runtime there are three main categories of activity:

1. Bootstrapping and opening databases.
2. Handling inbound socket requests from peers, miners, wallets, or local tools.
3. Maintaining outbound peer connections and performing synchronization.

## 2. Shared Runtime State

`node.py` creates a `Node` instance from `libs/node.py` and stores the live process state there. This object is the shared coordination point for the rest of the process.

Important fields on the shared node object:

- `db_lock`: global lock used to serialize block digestion and ledger mutation.
- `hdd_block` and `hdd_hash`: last block persisted to disk.
- `last_block` and `last_block_hash`: current chain tip known by the process.
- `peers`: `Peers` manager instance.
- `plugin_manager`: loaded plugins and hooks.
- `apihandler`: extra API command router.
- `syncing`: list of peers currently involved in sync work.
- `IS_STOPPING`: process-wide shutdown flag.

The design is not fully object-oriented. `node.py` is still the main orchestrator, and many modules read or mutate the shared node state directly.

## 3. Startup Sequence

The boot flow in `node.py` is:

1. Create runtime objects:
   - `node.Node()`
   - `logger.Logger()`
   - `keys.Keys()`
2. Load config with `options.Get().read()`.
3. Copy config values onto the shared `node` object.
4. Initialize logging.
5. Load plugins through `plugins.PluginManager`.
6. Determine network mode with `setup_net_type()`:
   - mainnet
   - testnet
   - regnet
7. Load the local wallet keys with `load_keys()`.
8. Open the Heavy3 mining data file with `mining_heavy3.mining_open()`.
9. Create long-lived managers:
   - `Peers`
   - `ApiHandler`
   - `Mempool`
10. Run database integrity checks and bootstrap from archive if needed.
11. Create the initial `DbHandler`.
12. Check ledger and hyperblock heights and optionally recompress hyperblocks.
13. Optionally clone the active ledger into RAM.
14. Initialize current chain tip state from the database.
15. Run initial checks:
   - bootstrap/fresh sync checks
   - sequencing checks
   - full ledger verification if enabled
   - index creation
16. Start the inbound TCP server unless Tor mode disables it.
17. Start the outbound `ConnectionManager` thread.
18. Enter the final keepalive loop until shutdown.

## 4. Network Modes

The node supports three network modes:

- Mainnet: default production behavior.
- Testnet: alternate port, ledger files, and peer files.
- Regnet: local controlled testing network with special `regtest_*` commands.

`setup_net_type()` rewrites paths, ports, allowed protocol versions, and some consensus behavior based on the selected mode.

## 5. Storage Layout

The node uses multiple SQLite databases at the same time.

### Main databases

- `ledger.db`: full ledger on disk.
- `hyper.db`: compressed chain representation used for hyperblock mode.
- `index.db`: alias and token indexes.
- `mempool.db`: pending transactions, unless configured in RAM.

### Per-thread DB access

Every inbound handler thread and outbound worker creates its own `DbHandler`. That handler opens:

- `hdd` / `h`: disk ledger connection and cursor.
- `hdd2` / `h2`: hyperblock database connection and cursor.
- `conn` / `c`: active chain connection and cursor.
  - If `ram=False`, this points at `hyper.db`.
  - If `ram=True`, this points at the shared in-memory ledger clone.
- `index` / `index_cursor`: alias and token index database.

This is important to the overall design:

- Reads are mostly per-thread.
- Mutating chain state is serialized by `node.db_lock`.
- `digest.py` writes new blocks to the active chain connection first.
- `dbhandler.db_to_drive()` later copies those new rows to `ledger.db`, and to `hyper.db` as well when the active chain is running from RAM.

## 6. Threads and Concurrency Model

The node is multi-threaded.

### Long-lived threads

- Main thread:
  - bootstraps the node
  - keeps the process alive
  - coordinates shutdown
- TCP server thread:
  - created via `server.serve_forever()`
  - accepts inbound connections
- Connection manager thread:
  - runs every ~30 seconds
  - purges mempool periodically
  - spawns outbound worker threads
  - logs status and consensus

### Per-connection threads

- Inbound request threads:
  - created by `socketserver.ThreadingMixIn`
  - one handler instance per inbound connection
- Outbound worker threads:
  - started by `Peers.client_loop()`
  - connect to remote peers and run the sync client protocol

### Important locks

- `node.db_lock`: only one block digestion or rollback-sensitive database mutation should run at a time.
- `mempool.lock`: serializes mempool writes and maintenance.
- `peers.peersync_lock`: serializes peer list merges.

The node does not attempt to make every shared structure lock-free or fully isolated. Instead, it protects a few critical paths and relies on per-thread database handles for the rest.

## 7. Socket Protocol

`connections.py` defines the wire protocol used by both inbound and outbound peers:

- The sender JSON-encodes the payload.
- A fixed 10-byte decimal header stores payload length.
- The receiver reads the header first, then reads exactly that many bytes.
- Timeouts return `"*"` in some flows and raise in others.

This protocol is used for:

- peer handshake
- block sync
- mempool exchange
- miner submission
- wallet and tooling commands
- `api_*` commands

## 8. Inbound Request Flow

The inbound server lives in `ThreadedTCPRequestHandler.handle()` inside `node.py`.

For each accepted connection, the handler:

1. Creates a fresh `DbHandler`.
2. Checks capacity against `thread_limit`.
3. Rejects banned peers.
4. Applies the `peer_ip` plugin filter.
5. Enters a loop calling `receive()` for commands.
6. Dispatches commands with a long `if/elif` command tree.

### Inbound command groups

The command surface mixes peer protocol, miner protocol, wallet helpers, and operational tooling.

#### Peer sync and protocol commands

- `version`
- `getversion`
- `hello`
- `sync`
- `sendsync`
- `blockheight`
- `blocksfnd`
- `nonewblk`
- `blocknf`
- `blocknfhb`
- `mempool`

These commands implement the core peer handshake and synchronization logic.

#### Block and mempool commands

- `block`
- `mpinsert`
- `mpget`
- `mpgetjson`
- `mpclear`

`block` is how miners or peers submit a mined block to the node.

#### Ledger and chain query commands

- `blocklast`
- `blocklastjson`
- `blockget`
- `blockgetjson`
- `balanceget`
- `balancegetjson`
- `balancegethyper`
- `balancegethyperjson`
- `difflast`
- `difflastjson`
- `diffget`
- `diffgetjson`
- `block_height_from_hash`

#### Address, alias, token, and utility commands

- `addlist`
- `listlim`
- `listlimjson`
- `addlistlim`
- `addlistlimjson`
- `addlistlimmir`
- `addlistlimmirjson`
- `aliasget`
- `aliasesget`
- `addfromalias`
- `aliascheck`
- `pubkeyget`
- `tokensget`
- `addvalidate`
- `annget`
- `annverget`
- `keygen`
- `keygenjson`

#### Status and control commands

- `statusget`
- `statusjson`
- `peersget`
- `portget`
- `stop`
- `addpeers`

#### API passthrough

Any command beginning with `api_` is delegated to `apihandler.ApiHandler`.

#### Plugin extension point

If no built-in command matches, the node checks plugin-provided command prefixes from `extra_commands`.

## 9. Outbound Peer Flow

Outbound connectivity is driven by `connectionmanager.py` plus `worker.py`.

### Connection manager

`ConnectionManager.connection_manager()` loops roughly every 30 seconds and:

- purges old mempool transactions periodically
- asks `Peers.client_loop()` to connect to candidates
- logs thread counts, sync status, mempool status, and consensus
- emits a plugin `status` hook

### Peer selection

`Peers.client_loop()`:

- shuffles known peers
- skips banned or recently retried peers
- limits multiple connections from the same C-class range
- respects thread limits
- starts a worker thread per selected peer

### Outbound worker session

`worker.worker()`:

1. Opens a socket to the remote peer.
2. Performs protocol handshake:
   - send `version`
   - receive `ok`
   - send `getversion`
   - receive peer version
   - send `hello`
3. Adds the peer to the active connection pool.
4. Creates its own `DbHandler`.
5. Enters a loop receiving remote commands and reacting to them.

The outbound side and inbound side speak the same protocol, but from opposite directions.

## 10. Peer Consensus Model

Consensus here is not the PoW validity rule itself. It is the node's view of what height most peers report.

`Peers.consensus_add()` stores each peer's reported height in `peer_opinion_dict`.

From that, the node derives:

- `consensus_most_common`: most common reported height
- `consensus_max`: highest reported height
- `consensus_size`: number of peers currently represented
- `consensus_percentage`: how aligned an individual peer is with the majority

This consensus view is used during sync decisions:

- If the local last block is old, the node prefers the most common height.
- If the chain appears current, the node prefers the longest chain.
- Large outliers can be warned or banned.

## 11. Block Sync Flow

The sync flow is split between `node.py` inbound handling and `worker.py` outbound handling.

### Typical sequence

1. Two peers handshake with `version`, `getversion`, and `hello`.
2. One side sends `sync`.
3. The other side sends `blockheight` and its current height.
4. Both sides compare heights and block hashes.
5. If the sender has blocks the receiver needs:
   - sender replies `blocksfnd`
   - receiver confirms with `blockscf`
   - sender sends one or more blocks
6. The receiver calls `digest_block()` to validate and apply them.
7. If hashes diverge and the common point is unknown:
   - sender may reply `blocknf` or `blocknfhb`
   - receiver may roll back below the disputed point
8. The session returns to `sync`.

### Why `blocknf` exists

`blocknf` and `blocknfhb` are rollback signals. They mean the remote peer could not find the advertised block hash in its own history, so local history may be on the wrong fork or too far ahead.

## 12. Block Processing Pipeline

Block processing is centralized in `digest.py`.

`digest_block()` is the critical write path:

1. Reject blocks from banned peers.
2. Acquire `node.db_lock`.
3. Wait for mempool writes to finish.
4. Build a `BlockProcessor`.
5. Process each block in the received batch.
6. Update checkpoints.
7. Persist changes.
8. Release locks and clean up.

### What is validated

For each block, the processor does the following:

- parses transactions into structured objects
- identifies the last transaction as the miner transaction
- validates transaction timestamps, addresses, amounts, and signatures
- checks duplicate signatures in the block and ledger
- computes live difficulty
- derives the new block hash from transactions plus the previous block hash
- verifies PoW with `mining_heavy3`
- checks spendability and fees
- calculates mining reward
- checks fork reward rules
- records token-related operations when present

### Commit path

New rows are inserted into the active chain database via `DbHandler.to_db()`. After that, the cleanup path copies new chain data to the disk ledger with `DbHandler.db_to_drive()`, also writes back to `hyper.db` when RAM mode is enabled, updates node height/hash fields, and removes mined transactions from the mempool.

## 13. Mempool Flow

`mempool.py` manages unconfirmed transactions.

Main responsibilities:

- create or open mempool storage
- accept direct tx inserts
- validate and merge inbound txs
- compute mempool-aware balances
- decide when a peer should receive a mempool diff
- purge stale transactions older than the configured age window

The mempool is used in three main ways:

1. `mpinsert` or `txsend` adds transactions.
2. `mempool` exchanges pending transactions with peers.
3. Balance queries include mempool debit impact when required.

The current implementation treats the mempool as a shared service inside the node process, not as a standalone component.

## 14. API Layer

`apihandler.py` handles commands that begin with `api_`.

This layer exists so that third-party tooling can ask richer questions without adding more special-case logic into the main protocol tree in `node.py`.

Examples include:

- `api_mempool`
- `api_getconfig`
- `api_getaddressinfo`
- `api_getblockfromhash`
- block listing and formatting helpers

The main node still owns the socket and thread; `ApiHandler` only handles the command-specific logic.

## 15. Plugins and Extension Points

`plugins.py` loads all plugin packages found under `./plugins` that contain an `__init__.py`.

The main node uses plugins in three ways:

### Filter hooks

Plugins can modify data before the node continues, for example:

- `peer_ip`
- `extra_commands_prefixes`

### Action hooks

Plugins can observe lifecycle events, for example:

- `init`
- `sync`
- `status`
- `mined`
- block-related hooks triggered during digestion

### Extra commands

Plugins can register additional command prefixes so inbound socket traffic can be routed outside the built-in `if/elif` tree.

This makes plugins the main extension mechanism without requiring deep edits to the core node.

## 16. Config-Driven Behavior

Important settings from `config.txt` that materially affect the node architecture:

- `port`: inbound server port
- `version` and `version_allow`: protocol compatibility
- `thread_limit`: overall concurrency cap
- `ledger_path` and `hyper_path`: main database files
- `full_ledger`: whether the full ledger is kept on disk
- `hyper_recompress`: whether hyperblocks may be recompressed
- `ram`: whether the active ledger runs from a RAM clone
- `accept_peers`: whether peer discovery updates are accepted
- `allowed`: which clients may call most commands
- `whitelist`: stronger trust set for selected commands
- `ban_threshold`: warning count before banning
- `tor`: disables the local server and uses SOCKS for egress
- `mempool_ram` and `mempool_path`: mempool storage mode
- `egress`: whether the node will serve blocks outward during sync

## 17. Shutdown Path

Shutdown is cooperative.

The `stop` command sets `node.IS_STOPPING = True` when sent from localhost. After that:

- new inbound work is rejected
- long-lived loops check the flag and exit
- the main loop waits for `db_lock` to be free
- Heavy3 resources are closed
- the process exits cleanly

`node_stop.py` exists as a helper script for this path.

## 18. Mental Model

The simplest accurate mental model for this codebase is:

- `node.py` is the orchestrator and protocol server.
- `Peers` decides who to talk to and what the network seems to agree on.
- `worker.py` is the outbound sync client.
- `digest.py` is the critical consensus enforcement path for incoming blocks.
- `dbhandler.py` hides the multi-database SQLite setup.
- `mempool.py` manages pending transactions and mempool relay.
- `apihandler.py` exposes richer external queries.
- `plugins.py` is the extension seam.

This means the "main node" is not just `node.py` in isolation. It is a hub process that coordinates network I/O, consensus checks, storage, and extension hooks through these modules.
