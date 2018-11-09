# How to implement Bismuth on an exchange

Some WIP notes to ease the job of wanna be exchanges.

# Prerequisites

## Python

BIS needs python 3.6+ and a few dependencies. It comes with classic requirements.txt files.
- its config is a config.txt in its dir.
- its chain data is several sqlite db in it's static subdirectory
- it can use an on disk mempool.db in its directory

## BIS is no BTC clone

It comes with specific features, but things you may be used to rely on are not available, by design.

- BIS transactions use no script, no UTXOs.  
- One wallet = one address
- No many-to-one, many-to-many, one-to-many transactions

## The repos

Main node code is at https://github.com/hclivess/Bismuth  
See https://github.com/hclivess/Bismuth/blob/master/_MOST_USEFUL_FILES.md  for a quickstart of what is what  

Quick install https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Install/Ubuntu_18.04_Install.md  
and FAQ https://github.com/bismuthfoundation/Bismuth-FAQ/

Most of the companion projects are under the BismuthFoundation organization or EggPool

- Native API Doc https://github.com/EggPool/BismuthAPI
- Bismuth plugins https://github.com/bismuthfoundation/BismuthPlugins  
- Json-RPC Server https://github.com/EggPool/BismuthRPC
- Console wallet demo https://github.com/bismuthfoundation/Antimony

## Official explorers

- http://bismuth.online (with API)
- https://hypernodes.bismuth.live/?page_id=83

# Interact with BIS

## Different levels

You can interact with BIS

- With the official explorer API  
  http://bismuth.online/apihelp
- With an extra - bitcoin alike - json-rpc server
- With the native API
- Straight by querying/feeding the DB

## The API

Native API Doc and code in several languages: https://github.com/EggPool/BismuthAPI  
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
  You can insert new tx in the mempool db directly

# Global Flow

## Deposits

Two choices

A/ One deposit address per user

Pros:
- Easier for the user

Cons:
- Many keys to create, handle and safe store
- requires large number of internal tx (with fees) to transfer between wallets  
  (large % of deposits are likely to end up on a different wallet)
- need to watch incoming blocks for all users addresses

B/ A single deposit address, and user specific id in the "data" (message) field of the transaction.

Pros:
- Single address to watch
- Single spending key to safe store
- No fees to transfer between wallets

Cons:  
- Users can forget the message.  
Workaround:
- We can hardcode your exchange address in the wallet, so they can't send to you without a message
- You can send back the empty messages funds after some time, run some garbage collector.

> We advise to use the B/ approach.

## Withdrawals

WIP

# Code examples

## Trigger an event on a specific transaction

See that exemple plugin for triggering an event on a specific transaction:
https://github.com/bismuthfoundation/BismuthPlugins/blob/master/plugins/201_on_transactions/__init__.py

## apt_getaddresssince

## Direct mempool insert

## Native API mempool insert

# Bismuth config

An excerpt of the important config items that may need tweaking in the context of running on an exchange.

WIP

# Regtest mode

https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/UnderTheHood/Regtest.md



