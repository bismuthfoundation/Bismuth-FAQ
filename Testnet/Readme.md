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

TODO: Miner setup doc coming soon.
