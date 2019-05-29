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

# FAQS

These will be amended with info from the Discord.

## Update script says "More than one commands.py or cron5.py found, exiting."
For instance, it says that and 
```
['/root/Bismuth', '/root/Bismuth-4.2.7']
['/root/hypernode/crontab']
```
This means you have 2 node installs, '/root/Bismuth' and '/root/Bismuth-4.2.7' and a single hypernode install at /root/hypernode.  
Regular install as done by the auto install script, and recommended dir architecture, is /root/Bismuth.  
Delete the extra node installs (make sur you backup what could be needed).  
Here, for instance: `rm -rd /root/Bismuth-4.2.7`  
Then restart the update script.

## When running the node: "inconsistent config, param is now mempool_ram in config.txt"

You kept the old config.txt file.  
Always use the distro config.txt, and put only what you need to override in config_custom.txt  
Unless you have a strong reason to tweak the config file, the default one is good.
Get the current config file:
```
cd ~/Bismuth
rm config.txt
wget https://raw.githubusercontent.com/bismuthfoundation/Bismuth/v4.3.0.0-beta.6/config.txt
```
