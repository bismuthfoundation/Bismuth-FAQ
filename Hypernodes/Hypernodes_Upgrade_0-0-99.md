# Bismuth Hypernode: Manual Upgrade to latest 0.0.99

0.0.99i5 is the current stable version.

**Hypernode Version 0.0.99 is likely to be a required update soon**  
In any case, this is a major stability and performance improvement ovver 0.0.98, so you have to update.

What's new:
- updated companion plugin
- more requests done by the plugin, with custom commands in the node context. Smoother requests, less db stress.
- completely rewritten mempool, faster and less cpu intensive
- improved sql db structure, faster
- improved handling of stalled peers and inner locks
- improved fork resistance and stability of PoW reference point.
- added temporary cache to alleviate some heavy computation pressure.
- recommended python version is now 3.7, fixes some async python bugs.

If it's a new install on a dedicated Ubuntu vps, use the one liner:  
`curl https://raw.githubusercontent.com/bismuthfoundation/hypernode/beta99/auto-install/bis-install.sh|bash`

If you have an auto install and want to auto upgrade, use the auto upgrade script:  
```
cd
rm hn_node_update.py
wget https://github.com/bismuthfoundation/util/raw/master/hn_node_update.py
python3 hn_node_update.py
```

The following is an overview for tech savy people.  


**Crucial steps are making sure your node is at latest 4.4.0.3 , companions updated, as well as running hn_check**

## What are the incidence on my rewards?

- If you follow the steps, you'll just get a few minutes down time, no impact
- At worst, say a full hour downtime, you'll just loose that hour (but others may, too, so same thing in the end)
- If you *don't* update, then you likely may fork and get stuck and get no more rewards.

## In a nutshell
(all takes place on the vps)

- Upgrade Python to 3.7
- Upgrade Bismuth Node to latest 4.4. branch
- install pre-requisites
- make sure to run `pip3 install -r requirements-node.txt` in Bismuth dir.
- Stop HN
- Update HN code to beta99 branch
- make sure to run `pip3 install -r requirements.txt`in hypernode dir 
- **Run hypernode/main/hn_check.py**
- Restart regular node
- Edit cron1.py to make sure it uses python3.7 instead of python3
- Check it works


## Install Python3.7

**Now required**

```
apt update
apt upgrade
apt install python3.7-dev python3-pip
```

Your default python still will be python3.6, you'll need to invoke 3.7 by `python3.7`  
To run pip, you'll now use `python3.7 -m pip ....`instead of `pip3 ...`

## Install pre-requisites

```
apt update
apt install build-essential libgmp3-dev unzip
```

libgmp and build-essential are required for fastecdsa module.

## Upgrade Bismuth Node to 4.4.x
(on the vps)

> I suppose you followed my advice and have bismuth installed under ~/Bismuth.  
If you have bismuth into Bismuth-4.x.x/... then it's time to change to ease future upgrades.

1. Fetch and extract the latest node code:

```
cd
rm master.zip
wget https://github.com/bismuthfoundation/Bismuth/archive/master.zip
unzip master.zip
```

This extracts the code to "Bismuth-master"

2. **Optionally**, make a backup:

```
mkdir Bismuth.bak
cp Bismuth/*.txt Bismuth.bak
cp Bismuth/*.der Bismuth.bak
```
(This includes your config.txt)

3. Update the node code  

`cp -av Bismuth-master/. Bismuth/`  
cleanup  
`rm -rd Bismuth-master`

**Impoertant**: Make sure you delete the old polysign directory:
```
cd 
cd Bismuth
rm -rd polysign
```

4. Optionnaly adjust your config.txt

(if you need specific setup, or this will be the default config)
`nano Bismuth/config.txt`

5. Add the required plugins and cleanup install

**Mandatory and important**

This manually installs the required plugins and companions.  
These steps suppose your bismuth dir is ~/Bismuth

```
cd 
cd Bismuth
cd plugins
mkdir 035_socket_client
cd 035_socket_client
rm __init__.py
wget https://github.com/bismuthfoundation/BismuthPlugins/raw/master/plugins/035_socket_client/__init__.py
cd ..
cd 500_hypernode
rm __init__.py
wget https://github.com/bismuthfoundation/hypernode/raw/beta99/node_plugin/__init__.py
cd 
cd Bismuth
rm ledger_queries.py
wget https://github.com/bismuthfoundation/hypernode/raw/beta99/node_plugin/ledger_queries.py
```

Make sure you have the correct dependencies:  
```
cd 
cd Bismuth
python3.7 -m pip install -r requirements-node.txt
python3.7 -m pip install ipwhois
```

5. Restart node with up to date code  
> `screen -ls` will list all your screens if you forgot the node screen name.  
I'll suppose it's "node"

`screen -x node`to enter the screen  
`ctrl-c` to kill the node  

`python3.7 node.py` to restart the node

Exit the screen `ctrl-a d`

## Upgrade Hypernode code
(on the vps)

You can leave your HN running while upgrading, you'll restart it at the end.

unzip is nice to have: `apt install unzip`  (already installed from the node requirements)

Upgrade the code:
- go in you home dir, where you installed the hypernode `cd`
- check the hypernode dir is there `ls -al` should list a "hypernode" directory
- rm any previous archive `rm beta99.zip`
- fetch the latest code `wget https://github.com/bismuthfoundation/hypernode/archive/beta99.zip`
- extract the code `unzip beta99.zip`
- copy the files over `cp -av hypernode-beta99/. hypernode/`
- this will not overwrite your custom config.txt nor the settings you may have in it
- cleanup  `rm -rd hypernode-beta99`
- go into the hn dir `cd hypernode`
- make sure the updated requirements are satisfied `python3.7 -m pip install -r requirements.txt`

- edit cron to use python3.7: `nano ~/hypernode/crontab/cron1.py`  
- edit the line `PYTHON_EXECUTABLE='python3'`so it reads `PYTHON_EXECUTABLE='python3.7'` instead, then save: ctrl-x, y, enter.

## Run the check 

- go into the main hypernode/main dir `cd;cd hypernode/main`
- run the check `python3.7 hn_check.py`  
- it should be happy.

## Restart your HN: 
- enter the hypernode screen `screen -x hypernode`
- stop the HN : ctrl-c
- you'll be kicked out of the screen, the cronjob sentinel will restart it in 1 minute.
- if you had no sentinel, restart it by hand.

# Upgrade FAQ

> To be completed with your questions.
