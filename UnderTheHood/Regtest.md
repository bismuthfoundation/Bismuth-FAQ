# Regtest mode support

Since 4.2.8 #3, bismuth nodes support a regtest mode.  
This mode is intended for dev, tests, and regression testing.

## Specific features

When launched in regtest mode:

- The node starts with a clean blockchain, with only a genesis block
- port is 3030
- difficulty is locked to an extremely low value, 16.
- the node does not connect to other nodes, does not accept other nodes connections.
- a "generate" command can be used to quickly generate new blocks, that embed txs from mempool if any.
- Apart from that, all the commands and node inner working stay the same

## The files

- ledger is static/regmode.db  
  It is emptied as each node start
- index is static/index_reg.db  
  It is emptied as each node start
- mempool is forced into RAM mode, whatever the config says.
- peers is peers_reg.txt and is empty
- suggested peers is also peers_reg.txt

## The config

In order to activate the regtest mode, add these lines at the end of your `config_custom.txt`:
```
port=3030
regnet=True
version=regnet
version_allow=regnet
```

## Usage

`python3 node.py` will run the node as usually.  
You'll see new messages regarding regnet.

Use `commands.py` to generate new blocks:  
`python3 commands.py generate 1`  
will generate one block.

The mining address that gets the block reward is the address from the local wallet.der file.
