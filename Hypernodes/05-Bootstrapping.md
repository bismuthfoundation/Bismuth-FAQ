# Bootstrapping Node and/or Hypernode

Bootstrapping allows you to get sync faster, with an up to date ledger from a safe source.

In order to bootstrap, the node or HN must be off, so you have to deactivate the cron sentinels first.  
We'll explain that.

The following instructions apply to the default auto install scheme.

## A. Node Bootstrap

### A1. Stop the node sentinel cron

`crontab -e`

You should have 
```
* * * * * cd /root/Bismuth;python3 node_sentinel.py
* * * * * cd /root/hypernode/crontab;python3 cron1.py
*/5 * * * * cd /root/hypernode/crontab;python3 cron5.py
```

Comment out the node sentinel, so it reads
```
#* * * * * cd /root/Bismuth;python3 node_sentinel.py
* * * * * cd /root/hypernode/crontab;python3 cron1.py
*/5 * * * * cd /root/hypernode/crontab;python3 cron5.py
```

ctrl-x, yes to save, enter

### A2. Stop the node

Clean stop request:  
`cd /root/Bismuth`  
`python3 commands.py stop`

check the node stopped:
`screen -x node`  
If there is no such screen, node is stopped.  
If not, you'll be in the node screen, it can use a few sec to stop.  
In case it does not want to stop clean, ctrl-c it, that will also close the screen.

### A3. Get the helper

`cd /root/Bismuth`  
`wget https://raw.githubusercontent.com/bismuthfoundation/util/master/snapshot_download.py`
`python3 snapshot_download.py`

Then follow the instructions.  
This will fetch and verify the bootstrap

### A4. Reactivate the node sentinel

`crontab -e`

Uncomment the node_sentinel line, so it reads again  
```
* * * * * cd /root/Bismuth;python3 node_sentinel.py
* * * * * cd /root/hypernode/crontab;python3 cron1.py
*/5 * * * * cd /root/hypernode/crontab;python3 cron5.py
```

The node will restart within a minute.

## B. HyperNode Bootstrap

WIP
