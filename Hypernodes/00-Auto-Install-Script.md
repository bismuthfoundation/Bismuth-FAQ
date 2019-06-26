# Auto install script for Bismuth Node + Hypernode, latest version.

## Pre-requisite

- An ubuntu 18.04 vps
- be ssh-connected as root to the vps (check your provider doc if this is new for you)

## One liner

Paste the following line, then hit 'enter':  
`curl https://raw.githubusercontent.com/bismuthfoundation/hypernode/master/auto-install/bis-install.sh|bash`

- The vps will then reboot, you'll lose the connection.  
- Wait 1 min then connect again

> Do not interrupt the process. Even if it may seem to be hanging for some time, let it go it will eventually move.

## Get your HN infos

`cd /root/hypernode/main`  
`python3 hn_check.py`

## Check your node live logs

`screen -x node`  
Will show the live logs of the node.  
Never close or interrupt, `ctrl-a d` will detach you from the live logs.

You can also get the current status without entering screen, by  
`cat /root/Bismuth/powstatus.json`

## Check your hypernode live logs

`screen -x hypernode`  
Will show the live logs of the hypernode.  
Never close or interrupt, `ctrl-a d` will detach you from the live logs.

## Check your hypernode status report

`cd /root/hypernode/main`  
`python3 hn_client.py --action=status --ip=1.2.3.4`  
> Replacing 1.2.3.4 by the real ip of your vps.

# Register your Hypernode

Please refer to https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Hypernodes/01-Hypernode_Registration.md
