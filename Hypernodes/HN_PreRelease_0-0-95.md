# Change log for Hypernodes v0.0.95

- Each file has its own version. The "Hypernode code" version I'm refering to will always be the version of the HN core, that is modules/poshn.py
- 0.0.95 is the pre-release. If tested ok, this will become the first real Hypernode release, 0.1.0

## What's New?

### Automatic Bootstrap

- When installing a new Hypernode, or in case of a massive error, the Hypernode will detect the lack of a pos chain and bootstrap from a dev controlled source.
- Bootstrap files building is automatic on the server side, every 4 hours, with a few previous versions archived for safety: You'll always get a fresh bootstrap.
- More bootstrap urls will be added later on, thanks to the "colored list feature" (see next features)

### Bugfixes and failsafes

- Several bugfixes, including a possible fix to stuck HN
- added crontab to restart Hypernode should it close
- added crontab to stop a frozen Hypernode

### Automatic Update

- Hypernodes now can receive and process update requests from a secure, dev owned source.
- Means you do not have to worry of updates, required updates will be pushed to your hypernode, he will fetch the up to date code and upgrade itself if needed.
- Of course, you can opt out of this feature if you prefer go the hard way.

### Companion plugin

- Hypernode code now comes with a companion plugin
- Auto installed to the Node directory
- This plugin adds features to the regular Bis nodes and allows for more coupling between PoW and PoS
- It's required.

### Colored lists

Usually, changes to the node core behaviour are done via code update. The dev pushes and update with a trigger block in the future; everyone is supposed to update (many do not in time and are then stuck), then the change or hard fork takes place later on.  
With the hypernodes, hark forks still will require a code update, but some tuning parameters can be adjusted by the devs also.  
For instance, in order to limit the bandwith used by the hypernodes, there is a max limit to the number of peers your Hypernode tries to connect to. This spares cpu and bandwith, for you and the whole HN net.  
But as the HN net grows, this setting may need to be adjusted so that you still can reach a high enough consensus.  
Same for the minimal consensus needed to forge a block.

These tuning parameters can now be adjusted network wide, on a new round, and will apply to every HN at the same time.  
They are sent from a trusted source on the PoW chain, dev controlled, and propagated via the companion plugin.

Several "colored" lists of parameters can be synced this way over the network, making it easier to react to an emergency situation or evolve some params in response to a significant network change, without any manual intervention from the Hypernodes owners.

## New config variables




## Hard Fork
A small Hark fork will be activated on Sept 13, 08:00 UTC.  
Until now, inactive Hypernodes still were participating in the juror slots election and could have a tickets, then a slot.  
After the fork, the inactive HNs are remove from the election.  

This does not change anything for the rewards: if your Hypernode was inactive and came back to live, he still will be considered as active no matter what.  
This just secures the PoS side a little more and makes sure all slots can be forged.

You **HAVE** to update to 0.0.95+ before that date or you may fork.

### 

