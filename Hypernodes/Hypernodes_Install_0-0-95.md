# THIS PAGE IS DEPRECATED

Please use the auto install script for new installs

https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Hypernodes/00-Auto-Install-Script.md


# Install Instructions, Hypernode v0.0.97

0.0.95 is the pre-release, pushed on 2018-09-11
0.0.97 is latest pre-release, pushed on 2018-10-02

If upgrading from an older version, see  
https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Hypernodes/Hypernodes_Upgrade_0-0-97.md instead

## Bismuth node

I suppose you already have a regular Bismuth node installed.  

If not, see https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Install/Linux.md  
or better, the quick version for Ubuntu 18 (*highly recommended*):  
https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Install/Ubuntu_18.04_Install.md

We'll suppose it lies in you home dir, then Bismuth  
`~/Bismuth`

> **Note**: We stongly encourage you to install the node in `Bismuth`dir and **not** in a version dependant directory.  
This will make updates and support way easier.

## OS Settings

Refer to https://github.com/EggPool/BismuthHowto/blob/master/Install/Linux.md#os-config

## Getting the Archive

move to your home dir and get the stuff  
```
cd
wget http://bp12.eggpool.net/hypernode.tar.gz
tar -zxvf hypernode.tar.gz
cd hypernode
```

## Python stuff

Python 3.6+ required  
Install prerequisites:  
`pip3 install -r requirements.txt`

If you have errors with that, do  
`apt install build-essential python3-dev`  
(prefix with sudo if not root)

Then `pip3 install wheel` , then again `pip3 install -r requirements.txt`  

If the requirements do not install, please get in touch on discord to tell what the message is.  
It can't work if the requirements fail.

## Config

`cd main`

If your bismuth install is not in the default directory, give the path:  
`nano config.txt`  (this will create a new file, no config.txt is in the release so it won't be changed by updates)
add a line:
`POW_LEDGER_DB=/path/to/the/ledger.db`

You can forget the config.txt if the install is standard.

## First check

run
`python3 hn_check.py`

This will
- create a new poswallet.json
- display some info
- check the ledger.db is readable

Example output:

```
address : "BrSKrY3mUyf9Cd9KYZqJmTgBnuT4Pt8xfn"
default_port : 6969
external_ip : "1.2.3.444"
running_instances : 0
POW_LEDGER_DB : "../../Bismuth-master/static/ledger.db"
```

Keep that info for future reference.  

*Note*: If you have `external_ip : "/bin/sh: 1: curl: not found"` message, do  
`sudo apt install curl`  
and retry the hn_check

> **Important**: hn_check also installs the companion plugin for the node.  
Therefore, once hn_check says it's ok, you **have** to restart the Bismuth node: go to your node screen, stop the node and restart it.

## Run the HN

Running in a 'screen' command could prove useful

```
screen -S hypernode
python3 hn_instance.py -v
```

`ctrl-a d` to detach, `screen -x hypernode` to reattach  
Make sure you don't run several nodes or screen. -S is only the first time, to *create* a screen. Then it's -x to attach to an existing screen.  
You can use `screen -ls` to list every one.

If the Hypernodes seems to run, then close it and setup the cronjob sentinel:
`screen -x hypernode` if you are not already in, ctrl-c to quit the hypernode, `exit` to close the screen

## Setup the Cronjob sentinel

The detailled steps are described here:
https://github.com/bismuthfoundation/hypernode/tree/master/crontab

This will handle auto restarts and make sure your HN works 100% of the time, unattended.

## Prepare some addresses
*This step is to be done on your local Wallet*.  
See faq below on how to create more addresses. 

* Create a new address for the collateral, send it exactly 10001 BIS (collateral + 1 bis for the txs fees)
* You create another address for the rewards, or use your current address for that purpose

## Register

Go to https://hypernodes.bismuth.live/?page_id=48

Your hypernode account address is the address given by hn_check.py from the earlier step, so is your ip address.  
This has to be done *from your dedicated collateral address* (it's a message to self), from your local wallet then.


- Get the reg bis url
- Paste in wallet - as is, "send" tab, "bis url" field. 
- click "load" button, it will prefill all fields so you can check what is does.  
- Hit "send" button to send the signed transaction and confirm.

If your HN is running, but the check says it's not, maybe your vps firewall is blocking.  
On ubuntu, use 
* `sudo ufw allow 6969` to open the HN port
* `sudo ufw allow 5658` to open the node port
(do both)

Please also store both reg and unreg bis url for future use (like raising your collateral)

## Wait

About 1h to 1h30 before activation, can take up to 3 hours, please be patient.
You can close the HN in between
(attach to the screen, ctrl-c)

> See the Hypernodes list page to check: https://hypernodes.bismuth.live/?page_id=163

# Notes

## Bismuth node wallet

Regular Bismuth node on the VPS, along with the HN:  
No constraint on the wallet: the wallet this node uses can be anything, do *NOT* put your collateral wallet there.

Your collateral wallet is only used to activate your HN, then it can be cold stored.  
Do not upload it ever.


# Some IRL questions

## Is it fine to put the BIS collateral address as the reward address?
It won't  bug, but it's better to use another address:  
- your collateral wallet stays cold
- no activity on the colateral address, safer and faster to check

## bis url: everything is pre-filled but amount is 0, not 1, is that ok?
No bis is harmed for registration.  
You send 0 bis, but some data, so there are a few fees.  
Why we said to have 1 BIS, so it covers for some TX should you make more later on

## POW_LEDGER_DB=/path/to/the/ledger.db is not found by hn_check?
Default works for a Bismuth install in ~/Bismuth.  
Maybe you have Bismuth installed under ~/Bismuth-4.2.6/  
Then in you config.txt, use `POW_LEDGER_DB=../../Bismuth-4.2.6/static/ledger.db`

## when running hn_check.py I get ModuleNotFoundError: No Module named 'whatever'
- check you have python 3.6.4 + via python3 --version
- make sure you run via python3, not just python
- no module named... => requirements.txt not ok: make sure you are in the top level dir, where the requirementS.txt file is, and rerun `pip3 install -r requirements.txt`. If you are in ~/hypernode/main, run `pip3 install -r ../requirements.txt`

## The logs scroll too fast, Can't see the line in red!
If you detach from the screen, you can also quietly look at the logs, logs dir under "main", and search for `[E` to get the errrors for instance.

## should I check daily that everything is ok?
No need to, unless you are stuck  
The logs rotate, they won't fill your disk up

## How long can I be missing before I get unregistered?
We are still polishing the reward logic, but granularity should be one hour.  
That is, if you're down less than one hour, you'll be as good as 100%  
Real unreg would only come after longer time, like half a day or one full day.

## On some VPS, when installing python3-pip
Error: E: unable to locate package python3-pip

```
sudo add-apt-repository main
sudo add-apt-repository universe
sudo add-apt-repository restricted
sudo add-apt-repository multiverse
```

then `sudo apt update` and retry

## On some VPS, when pip3 install: "No module named 'setuptools'"
`pip3 install setuptools`, then redo the pip3 install


## How do you generate a new address in the light wallet?
Store your wallet.der, or rename it to "my_precious_wallet.der" then run wallet again, it will generate a new wallet.der with a new address.  
Then from the wallet, you can load any renamed wallet.der  
wallet > load wallet and then choose whichever one you want


