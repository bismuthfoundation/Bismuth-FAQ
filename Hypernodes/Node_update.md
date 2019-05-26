# Bismuth Core Node Update

At block height 1,200,000 there will be a hardfork in the Bismuth PoW blockchain. This means that all hypernodes need to update their PoW node to the latest version. The script can also be used whenever you want to update your hypernode in the future to the latest core node PoW version, independent of the hardfork at block height 1,200,000.

To ease this process, an update script written in Python is made available. To run the update script, the following commands can be run in a terminal window:

```
rm hn_node_update.py
wget https://raw.githubusercontent.com/bismuthfoundation/util/master/hn_node_update.py
python3 hn_node_update.py
```

The update script can be run from any directory on your node. The script performs the following steps automatically:

1. Downloads the latest Bismuth node release from GitHub  
2. Asks if you want to take a backup copy of your existing config.txt  
3. Downloads the latest verified Bismuth ledger  
4. Stops all screen jobs
5. Takes a backup of your cron jobs  
6. Replaces your old ledger with the verified one  
7. Restarts the Bismuth node  
8. Restores the cron jobs which will, after a short delay, restart your hypernode  

If anything goes wrong during the node update, please leave your comments in the Discord hypernodes channel, https://discord.gg/4tB3pYJ
