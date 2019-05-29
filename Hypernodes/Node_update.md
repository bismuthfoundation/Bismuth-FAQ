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

## First run, the command "rm hn_node_update.py" says "rm: cannot remove 'hn_node_update.py': No such file or directory"

That's ok. This is to deleted potential older file, this means there was no previous update script, this is cool. just go on.

## Update script says "More than one commands.py or cron5.py found, exiting."
For instance, it says that and 
```
['/root/Bismuth', '/root/Bismuth-4.2.7']
['/root/hypernode/crontab']
```
This means you have 2 node installs, '/root/Bismuth' and '/root/Bismuth-4.2.7' and a single hypernode install at /root/hypernode.  
Regular install as done by the auto install script, and recommended dir architecture, is /root/Bismuth.  
Delete the extra node installs (make sure you backup what could be needed).  
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

## After the update looks like my hypernode is started but I don't see the Node screen. Do I have to do something to restart the node? Or at least to get the node on it's own screen?

If `screen -ls` does not show a "node" screen, then the node is not running.  
Make sure, try running it by hand: maybe something is preventing him from running, then you'll know
```
cd ~/Bismuth
screen -S node
python3 node.py
```
Will create a new "node" screen, do this only once.

## I have several HN installs or several HN wallets, how do I know which one is the right one?

Your pos (hn) wallet should be the one your registered. If you change HN address, you'll have to unregister the old one, wait a few hours, the re-register the new one. Keep the same address to avoid the pain.  
You can see the content of a pos wallet via the "cat" command.  
Default location: `cat ~/hypernode/main/poswallet.json` this is a json file, with "address" field inside.

This is also the file you need to backup if you want to do a full - clean - reinstall from the auto install script.
