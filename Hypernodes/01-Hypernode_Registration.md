# Hypernode Registration

You Hypernode is up and running, now what?


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


# Some IRL questions

## Is it fine to put the BIS collateral address as the reward address?
It won't  bug, but it's better to use another address:  
- your collateral wallet stays cold
-  activity on the colateral address, safer and faster to check

## bis url: everything is pre-filled but amount is 0, not 1, is that ok?
No bis is harmed for registration.  
You send 0 bis, but some data, so there are a few fees.  
Why we said to have 1 BIS, so it covers for some TX should you make more later on

## How long can I be missing before I get unregistered?
We are still polishing the reward logic, but granularity should be one hour.  
That is, if you're down less than one hour, you'll be as good as 100%  
Real unreg would only come after longer time, like half a day or one full day.

## How do you generate a new address in the light wallet?
Store your wallet.der, or rename it to "my_precious_wallet.der" then run wallet again, it will generate a new wallet.der with a new address.  
Then from the wallet, you can load any renamed wallet.der  
wallet > load wallet and then choose whichever one you want.  
Tornado wallet has integrated multiaddress management.

