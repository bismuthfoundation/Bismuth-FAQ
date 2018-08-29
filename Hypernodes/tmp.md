# Temp Instructions

For testing purposes, will evolve in the upcoming days.

## Bismuth node

I suppose you already have a regular Bismuth node installed.  
We'll suppose it lies in you home dir, then Bismuth  
`~/Bismuth`

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

## Run the HN

Running in a 'screen' command could prove useful

```
screen -S hypernode
python3 hn_instance.py -v
```

`ctrl-a d` to detach, `screen -x hypernode` to reattach

## Register

Go to https://hypernodes.bismuth.live/?page_id=48

Your hypernode account address is the address given by hn_check, so is your ip address.


- Get the bis url
- Paste in wallet - as is, "send" tab, "bis url" field. 
- click "load" button, it will prefill all fields so you can check what is does.  
- Hit "send" button to send the signed transaction and confirm.

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

```sudo add-apt-repository main
sudo add-apt-repository universe
sudo add-apt-repository restricted
sudo add-apt-repository multiverse```

then `sudo apt update` then retry
