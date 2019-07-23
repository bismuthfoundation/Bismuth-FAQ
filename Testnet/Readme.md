Bismuth has a testnet which is used for testing and debugging purposes by the core team. Developers are also recommended to test their dApps on testnet before releasing on mainnet.

This document describes how to setup and run a testnet node on Linux, assuming python version 3.6 or later.

If you want to run a stable testnet node, download and unzip the source code from here: [Bismuth Releases](https://github.com/hclivess/Bismuth/releases)

If you want to test the latest code under development, execute the following command:   
```git clone https://github.com/hclivess/Bismuth.git```

If you have previously cloned the Bismuth repo, update it with:   
```git pull```

In the ~/Bismuth folder, edit the config.txt file and set the following parameters:   
```
port=2829
version=testnet
version_allow=testnet
testnet=True
debug=True
debug_level=WARNING
```

If the file peers_test.txt does not exist, create one with the following contents:   
```
{'35.227.90.114': '2829','51.15.97.143': '2829','78.28.227.89': '5658','163.172.163.4': '2829','94.113.207.67': '2829'}
```

The testnet node can then be started with, for example:   
```
cd ~/Bismuth
screen -mS node python3 node.py
```

When starting a testnet node, the following question will appear:   
```Status: Welcome to the testnet. Redownload test ledger? y/n```

If you answer "y" here, the testnet ledger will be downloaded from https://bismuth.cz   
Sometimes, the ledger from bismuth.cz can be far behind the current block height. Alternatively, if you want to get a testnet node synced as fast as possible, you can download a recent snapshot from here: https://hypernodes.bismuth.live/?page_id=207   

Exit the screen mode by ```Ctrl-a d```

Then stop the node using: ```python3 commands.py stop```

The following commands will download and unpack a recent testnet snapshot:   
```
cd ~/Bismuth/static
wget http://51.15.97.143/snapshots/testledger-870000.tar.gz
tar -zxvf testledger-870000.tar.gz
```

Replace the number 870000 in the example above with the latest snapshot height found at: https://hypernodes.bismuth.live/?page_id=207   

After downloading and unpacking the snapshot, start the testnet node again and just answer "n" when it asks "Redownload test ledger".

To check that the testnet node has started properly and the current block height, exit the screen mode by ```Ctrl-a d```

Then type: ```python3 commands.py statusget```

To go back to the screen mode, type: ```screen -r node```

If you need some testnet BIS for your own dApp development, ask in the #testnet channel on discord. Alternatively, you can setup your own (cpu) miner and mine your own testnet BIS.

To cpu mine on testnet, you first need to run a pool. The open source optipoolware.py is recommended.   
```
git clone https://github.com/maccaspacca/Optipoolware.git
```
Edit the file pool.txt and define:   
```
mine_diff=50
min_payout=1000
```
Copy the following files into the ~/Bismuth folder:   
```
optipoolware.py
pool.txt
```
The pool can be started with:   
```
cd ~/Bismuth
screen -mS pool python3 optipoolware.py
```

Next, edit the file optihash/miner.txt:   
```
mining_threads=1
```

Copy the following files into the ~/Bismuth folder (at the ~/Bismuth level, not in a new sub-directory "optihash"):   
```
optihash/optihash.py
optihash/miner.txt
```

The miner can be started with:   
```
cd ~/Bismuth
screen -mS miner python3 optihash.py
```

When a new block is mined by your cpu miner, the testnet reward is paid to the account shown in the "address" field of the file wallet.der.
