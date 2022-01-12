# Auto install script for Bismuth Node + Hypernode, Newest version for Ubuntu 20.04 LTS.


## A. Pre-requisite and Hardware VPS Requirements

- An **Ubuntu 20.04 LTS** vps. *Do not use a higher version, stick to LTS*
- be ssh-connected as root to the vps (check your provider doc if this is new for you)

The auto install script will add a 3GB swap file. Minimal requirements are **2GB real RAM**.  
You can try with 1GB Ram only, it runs but we make no guarantee on the long term or metrics of your HN.

**2 Cores/vCPUs** are the official requirements.  
You can try with a single core, but again, this is not officially supported and you're likely to run into troubles at some point.

At least **50gb** SSD are currently advised. Hard drive should also work.

> Your vps provider also plays a huge role. I saw bad hosts with several vCpus performing way worse than top notch 1cpu vps. If you have to choose, prefer CPU over RAM. (like, better 2 cpu/2GB ram than 1 cpu/4Gb ram)

**Note**: This script is intended to run on a *blank* vps, with no other apps running, with default ssh on port 22. UFW Firewall will be activated with a custom set of rules. If you are in a specific setup or need more open ports, please have a look at the script source and adjust accordingly.

## B. One liner

Paste the following line, then hit 'enter':  
`curl https://raw.githubusercontent.com/bismuthfoundation/hypernode/master/auto-install/bis-install-u20.sh|bash`

- The server will do its things for a few minutes
- The vps will then reboot, you'll lose the connection.  
- Wait 1 min then connect again

> Do not interrupt the process. Even if it may seem to be hanging for some time, let it go it will eventually move.

Once you can connect again, be a little more patient. Rushing and restarting things often do break.  
You can issue a `screen -ls` command after 1 minute to make source node and HN are started.  
If the command lists both a "node" and a "hypernode" screen, you're ok.

Then, **let the server alone for 30 minutes.**  
Both node and HN have to init, discover peers, catch up... let'em play.

> This version uses python3.8, which is the system Python, that is invoked implicitely with "python3".  If you see old tutos with "python3.7 xxx", just do "python3 xxx" instead.

## C. Get your HN infos

`cd /root/hypernode/main`  
`python3 hn_check.py`

Backup your PoS wallet, that'll be useful if you need to reinstall your HN without messing with registration again:

`cat /root/hypernode/main/poswallet.json`

Copy the outpout in a text file (all goes on a single line) for backup.

# Optional

## D. Check your node live logs

`screen -x node`  
Will show the live logs of the node.  
Never close or interrupt, `ctrl-a d` will detach you from the live logs.

You can also get the current status without entering screen, by  
`cat /root/Bismuth/powstatus.json`

## E. Check your hypernode live logs

`screen -x hypernode`  
Will show the live logs of the hypernode.  
Never close or interrupt, `ctrl-a d` will detach you from the live logs.

## F. Check your hypernode status report

`cd /root/hypernode/main`  
`python3 hn_client.py --action=status --ip=1.2.3.4`  

> Replacing 1.2.3.4 by the real ip of your vps.

# Register your Hypernode

Please refer to https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Hypernodes/01-Hypernode_Registration.md
