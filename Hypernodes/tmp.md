# Temp Instructions

DO NOT USE YET, For team purposes only.

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
wget url_of_the_tar.gz
tar -zxvf hypernode.tar.gz
cd hypernode
```

## Python stuff

Python 3.6+ required  
Install prerequisites:  
`pip3 install -r requirements`

## Config

`cd main`

If your bismuth install is not in the default directory, give the path:  
`nano config.txt`
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

Your hypernode account address is the address given by hn_check, so is your ip address

Get the bis url

paste in wallet, load, send

## Wait

About 1h to 1h30 before activation.
You can close the HN in between
(attach to the screen, ctrl-c)

# Notes

## Bismuth node wallet

Regular Bismuth node on the VPS, along with the HN:  
No constraint on the wallet: the wallet this node uses can be anything, do *NOT* put your collateral wallet there.

Your collateral wallet is only used to activate your HN, then it can be cold stored.  
Do not upload it ever.
