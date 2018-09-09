# My Hypernode shows as "Inactive"

My hypernode shows as "Inactive" on https://hypernodes.bismuth.live/?page_id=163  
What should I do?

## Reasons

- Can be stuck on "new round" (known bug)
- Can be down / stopped
- Can be stuck for another reason

## What to do

### Check it's running
Log in to your VPS, go to your "hypernode" screen: `screen -x hypernode` ,  check the output.  
Or, you can alternatively use the hn_client util to get more info, see https://github.com/bismuthfoundation/Hypernode-Alerts#hypernode-native-client

### It's down 
If it's down, obviously restart it.

### It's stuck, last logs are not current
Close it, then restart it

## Pre-emptive cure
If you feel comfortable with crontabs, see https://github.com/bismuthfoundation/hypernode/tree/master/crontab

There are cronjobs to monitor your HN and restart it, should it close.

## Monitoring Alerts
See https://github.com/bismuthfoundation/Hypernode-Alerts for possible real time alert solutions (WIP).
