# How to implement Bismuth on an exchange

Some WIP notes to ease the job of wanna be exchanges.

# Prerequisites

## Python

BIS needs python 3.6+ and a few dependencies. It comes with classic requirements.txt files.
- its config is a config.txt in its dir.
- its chain data is several sqlite db in it's static subdirectory
- it can use an on disk mempool.db in its directory

> **Make sure your system supports utf-8 locales** or you'll likely run into issues.

## BIS is no BTC clone

It comes with specific features, but things you may be used to rely on are not available, by design.

- BIS transactions use no script, no UTXOs.  
- One wallet = one address
- No many-to-one, many-to-many, one-to-many transactions

## The repos

Main node code is at https://github.com/bismuthfoundation/Bismuth  
See https://github.com/bismuthfoundation/Bismuth/blob/master/_MOST_USEFUL_FILES.md  for a quickstart of what is what  

Quick install https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Install/Ubuntu_18.04_Install.md  
and FAQ https://github.com/bismuthfoundation/Bismuth-FAQ/
**Always use the latest stable release**

**New:** An auto install script, node alone, is provided: https://github.com/bismuthfoundation/Bismuth/tree/master/auto-install

Most of the companion projects are under the BismuthFoundation organization or EggPool

- Native API Doc https://github.com/bismuthfoundation/BismuthAPI  
- Bismuth plugins https://github.com/bismuthfoundation/BismuthPlugins  
- Json-RPC Server https://github.com/EggPool/BismuthRPC  
- Console wallet demo https://github.com/bismuthfoundation/Antimony

## Official explorers

- https://bismuth.im (with API)  
- https://bismuth.online (with API)  
- https://hypernodes.bismuth.live/?page_id=83

# Interact with BIS

## Different levels

You can interact with BIS

- With the official explorer API  
  https://bismuth.online/apihelp  
- Fallback explorer API  
  https://bismuth.im/apihelp  
- With an extra - bitcoin alike - json-rpc server  
- With the native API   
- Straight by querying/feeding the DB

## The API

Native API Doc and code in several languages: https://github.com/bismuthfoundation/BismuthAPI  
Prefered way to interact with Bismuth nodes. No extra layer, direct feedback.

## The Json-RPC Server

Extra layer designed to be as close as a bitcoin rpc as possible. Given the difference with BTC, a 1:1 interface is not achievable.  
Released quite some time ago, but was never used in a production environment until now.

https://github.com/EggPool/BismuthRPC  
https://github.com/EggPool/BismuthRPC/blob/master/RPCServer/Commands.md

## Direct DB Access

Doc and structure to be released if needed.

- `static/ledger.db`: blockchain  
- `static/hyper.db`: pruned chain  
- `static/index.db`: additionnal index db  
- `mempool.db`: to be embedded transactions.   
  You can insert new tx in the mempool db directly. Example below.

# Global Flow

## Deposits

Two choices

A/ One deposit address per user

Pros:
- Easier for the user

Cons:
- Many keys to create, handle and safe store  
- Requires large number of internal tx (with fees) to transfer between wallets  
  (large % of deposits are likely to end up on a different wallet for withdrawal).  
  Dust + fees  
- Needs to watch incoming blocks for all users addresses  

B/ A single deposit address, plus user specific id in the "data" (message) field of the transaction.

Pros:
- Single address to watch  
- Single spending key to safe store  
- No fees to transfer between wallets  

Cons:  
- Users can forget to paste the message.  

Workaround:  
- We can hardcode your exchange address in the wallet, so they can't send to you without a message  
- You can send back the empty messages funds after some time, run some garbage collector.  

> We advise to use the B/ approach.

## Withdrawals

Depending on the exchange flow, you can have a spending script assemble, sign and send the tx to the node (via DB or tcp socket) or (safer) assemble an unsigned transaction, have a secure node just sign it, and let anyone send it to the network.

> See send_nogui below

## Bismuth TXIDs

TXIDs are the first 56 chars of the transaction signature (a base64 encoded buffer).  
You can then have a TXID before you really *send* the transaction to the network.

See https://github.com/bismuthfoundation/Bismuth/blob/master/check_tx.py  
A Demo script that takes a transaction id as input, and sends back a json with its status.  
Comes with a doc https://github.com/bismuthfoundation/Bismuth/blob/master/check_tx.md

# Code examples

## commands.py

https://github.com/bismuthfoundation/Bismuth/blob/master/commands.py

Basic console client to interact with a node.  
You can use the same commands via the native Bismuth API https://github.com/bismuthfoundation/BismuthAPI

ex:

- `python3 commands.py stop`  
  clean stops the node, avoids breaking the db
- `python3 commands.py balancegetjson 9ba0f8ca03439a8b4222b256a5f56f4f563f6d83755f525992fa5daf`  
  returns the detailled balance of that address, as json dict:  
 `{"balance": "102.29444000", "credit": "105.99643000", "debit": "102.29444000", "fees": "102.29444000", "rewards": "102.29444000", "balance_no_mempool": "102.29444000"}`  


## send_nogui.py

Command line script to assemble, sign, send a transaction.  
Can be used as base for automated payment scripts.  
Does not send anything sensitive to the node.

https://github.com/bismuthfoundation/Bismuth/blob/master/send_nogui.py

This script can be split into several functional parts

- connects to local mempool db to check for un-mined tx  
- connects to local ledger to check for balance  
- calculates fees  
- assemble transaction structure  
- signs transaction  
- sends a "mpinsert" message via tcp socket to the local node.  
- prints the node answer.

The first steps may be avoided: You can just ignore the local db checks: the local node itself will answer with the matching error message if you spent too much because of a mempool tx.

The critical thing to keep is the very strict format of the transaction to be signed, and signed.  
Like, timestamp as .2f string , str(transaction) with transaction a tuple, not a list...  
Since the nodes will re-assemble the tx to check the sig, any meaningless char that would change will break.

> We can provide variants or parts of this script suited to your precise use case, just ask.


## Trigger an event on a specific transaction

Bismuth plugins can be used to trigger an event (like http hook or anything) as soon as a block is digested by the node.  
See that exemple plugin for triggering an event on a specific transaction:  
https://github.com/bismuthfoundation/BismuthPlugins/blob/master/plugins/201_on_transactions/__init__.py


## api_getaddresssince

This new node command allows to poll a node for the new transactions related to a specific address, since a given checkpoint block, with a number of confirmations.

See demo script for usage: https://github.com/bismuthfoundation/Bismuth/blob/master/demo_getaddresssince.py

## Get node status

This demo scripts shows how to query the node and get a json back, will consensus, connectivity, bocks and version info:  
https://github.com/bismuthfoundation/Bismuth/blob/master/demo_getstatus.py


# Bismuth config

See https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Nodes/Node_config.MD

Here is an excerpt of the important config items that may need tweaking in the context of running on an exchange.  
Non listed params have to stick to the defaults.

- "version" and "version_allow" are Hard fork related
- "thread_limit" is the number of possible clients to accept. 24 is the default.  
  You can raise to 50 to have a wider connectivity.
- "allowed" is the list of ips allowed to execute specific commands, like the wallet functions, mpinsert...  
  You want to remove "any" from that list and only list your own safe ips there.
- "reveal_address" You probably want to set to False if you don't want your node ip/address to be linked.  
  You can let to "True" if your node runs on an unused address (advised)
- "whitelist": Having a few team operated nodes and pool nodes in your whitelit definitely can help stay synced. Ask us.
- "mempool_allowed" is the list of address to accept txs from, even if the mempool is full.
- set "terminal_output" to True to get more console feedback
- set "ram" to False to use disk rather than ram and spare some Memory (default, safer)
- set "mempool_ram" to False if you plan to access the mempool db from scripts.

# Extra plugin

Some large scale attacks come from some specific IPS or cloud services.  
We strongly advise to run the hypernode plugin on your node. Although designed for the Hypernodes, it does not require one, and adds the following features:

- powstatus.json file with current state of the node  
- ip filtering of peers based upon on chain synced black and whitelists

Just add the plugin file in a plugins/500_hypernode/ directory of your Bismuth install.  
https://github.com/bismuthfoundation/hypernode/blob/master/node_plugin/__init__.py  

Needs 
- `pip3 install dnspython3`
- `pip3 install ipwhois`

# Regtest mode

Bismuth now comes with a regtest mode

https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/UnderTheHood/Regtest.md

# Docker images

Bismuth node and rpc-server now come as a Docker image, see 
- Node https://github.com/bismuthfoundation/Bismuth-Docker/tree/master/node
- RPC Server https://github.com/bismuthfoundation/Bismuth-Docker/tree/master/rpc-server



