# Bismuth Hypernode: Upgrade to 0.0.95

See what's new: https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Hypernodes/HN_PreRelease_0-0-95.md

If it's a new install, see: https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Hypernodes/Hypernodes_Install_0-0-95.md

> **Ongoing** , please wait ;)

## What are the incidence on my rewards?

- If you follow the steps, you'll just get a few minutes down time, no impact
- At worst, say a full hours downtime, you'll just loose that hour (but others may, too, so same thing in the end)
- If you *don't* update, then you likely may fork and get stuck and get no more rewards.

## In a nutshell

- Stop HN
- Update code
- Run hn_check
- Restart regular node
- Install cronjob sentinel
- Check it works

## Upgrade code

Stop your HN: 
- log in to your server, 
- if you had already cronjobs configured, deactivate them for the upgrade
- enter the hypernode screen `screen -x hypernode`
- stop the HN : ctrl-c
- exit the screen `exit`

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
- restart the regular node:  enter the node screen, stop and restart the regular node.
- at launch, the node will tell "Init Hypernode plugin"
- exit the node screen
- make sure the hypernode is running, from hypernode/main: `python3 hn_instance.py -v`
- if it runs, you can close it

## Setup the cronjob sentinel

The detailled steps are described here: https://github.com/bismuthfoundation/hypernode/tree/master/crontab

This will handle auto restarts and make sure your HN works 100% of the time, unattended.  

Try `screen -ls` until you see a "hypernode" screen listed, it works!

# Upgrade FAQ

> To be completed with your questions.
