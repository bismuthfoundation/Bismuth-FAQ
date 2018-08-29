# Install log on an ubuntu server 18.04 Machine

Add `sudo` in front of the apt install commands if your main user is not root.
These are all the commands I had to type to install a node on a brand new stock server.

## First login

- Change pass or config to use certificates instead of password (advanced user)
- `apt update` àpt upgrade` `apt dist-upgrade`
- reboot

## Check prerequisites

- `python3 --version` : should report > 3.6.4. I have 3.6.5
- install pip: `apt install python3-pip`
- install sqlite3: `apt install sqlite3`

## Get node code

```
cd
wget https://github.com/hclivess/Bismuth/archive/4.2.6.tar.gz
tar -zxvf 4.2.6.tar.gz
```
rename to Bismuth: `mv Bismuth-4.2.6 Bismuth`

## Install node dependences

- `cd Bismuth`
- `pip3 install -r requirements-node.txt`
- `clear` because it broke my terminal

## Run in a screen

- `screen -S BIS_NODE`
- `python3 node.py`

Shows: Database needs upgrading, bootstrapping...  
Downloaded 0 %  
Downloaded 1 %  
...  

Then begins to test chain coherence, moves to ram, and syncs.

## Screen management

- ctrl-a d to detach and log out
- screen -x BIS_NODE to re-attach to this screen and check node output.
- ctrl-a d to detach again
