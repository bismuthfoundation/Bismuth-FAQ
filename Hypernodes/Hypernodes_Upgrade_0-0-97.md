# Bismuth Hypernode: Upgrade to 0.0.97

**Hypernode Version 0.0.97 is a required update before Sunday Oct 7, 14h UTC**

What's new:
- more checks
- updated companion plugin
- makes sure the regular node it up to date for PoW fork

If it's a new install, see: https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Hypernodes/Hypernodes_Install_0-0-95.md  
(same steps, will install 0.0.97).

**Crucial steps are making sure your node is at least 4.2.7 , as well as running hn_check**



## What are the incidence on my rewards?

- If you follow the steps, you'll just get a few minutes down time, no impact
- At worst, say a full hour downtime, you'll just loose that hour (but others may, too, so same thing in the end)
- If you *don't* update, then you likely may fork and get stuck and get no more rewards.

## In a nutshell
(all takes place on the vps)

- Upgrade Bismuth Node to 4.2.7
- Stop HN
- Update code
- **Run hn_check**
- Restart regular node
- Install cronjob sentinel if needed
- Check it works

## Upgrade Bismuth Node to 4.2.7
(on the vps)

> I suppose you followed my advice and have bismuth installed under ~/Bismuth.  
If you have bismuth into Bismuth-4.2.6/... then it's time to change to ease future upgrades.

1. Fetch and extract the latest node code:

```
cd 
wget https://github.com/hclivess/Bismuth/archive/4.2.7.tar.gz
tar -zxvf 4.2.7.tar.gz
```

This extracts the code to "Bismuth-4.2.7"

2. **Optionally**, make a backup:

```
mkdir Bismuth.bak
cp Bismuth/*.py Bismuth.bak
cp Bismuth/*.txt Bismuth.bak
cp Bismuth/*.der Bismuth.bak
```

3. Update the node code  

`cp Bismuth-4.2.7/*.py Bismuth`

4. Double check your config.txt

`nano Bismuth/config.txt`

The 3 important things to check are:

```
version_allow=mainnet0017,mainnet0018,mainnet0019
terminal_output=True
quicksync=False
```

Change these lines if they say otherwise.

5. Restart node with up to date code  
> `screen -ls` will list all your screens if you forgot the node screen name.  
I'll suppose it's "node"

`screen -x node`to enter the screen  
`ctrl-c` to kill the node  
`python3 node.py` to restart the node.

It will say "Creating Junction Noise file, this usually takes a few minutes..."   
This means at least 3 minutes.  
Do not interrupt, let it create the file and sync.

Once the node is sync, stop it by ctrl-c

Exit the screen `ctrl-a d`

## Upgrade Hypernode code
(on the vps)

You can leave your HN running while upgrading, you'll restart it at the end.

Upgrade the code:
- go in you home dir, where you installed the hypernode `cd`
- check the hypernode dir is there `ls -al` should list a "hypernode" directory
- rm any previous .tar.gz archive `rm hypernode.tar.gz`
- fetch the latest code `wget http://bp12.eggpool.net/hypernode.tar.gz`
- extract the code `tar -zxvf hypernode.tar.gz`
- this will not overwrite your custom config.txt nor the settings you may have in it
- go into the hn dir `cd hypernode`
- make sure the updated requirements are satisfied `pip3 install -r requirements.txt`

## Run the check 

- go into the main hypernode/main dir `cd main`
- run the check `python3 hn_check.py`

## Restart the node

- `screen -x node`
- `python3 node.py`

## Restart your HN: 
- enter the hypernode screen `screen -x hypernode`
- stop the HN : ctrl-c
- you'll be kicked out of the screen, the cronjob sentinel will restart it in 1 minute.
- if you had no sentinel, restart it by hand.

## Setup the cronjob sentinel

(no need to change if you already did it)

The detailled steps are described here: https://github.com/bismuthfoundation/hypernode/tree/master/crontab

This will handle auto restarts and make sure your HN works 100% of the time, unattended.  

Try `screen -ls` until you see a "hypernode" screen listed, it works!

# Upgrade FAQ

> To be completed with your questions.
