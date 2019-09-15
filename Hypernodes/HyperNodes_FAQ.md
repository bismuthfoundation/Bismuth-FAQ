# Bismuth HyperNodes FAQ

2019-07-27: v 0.0.99 Release, https://github.com/bismuthfoundation/hypernode/releases
(...)
2018-09-11: V 0.0.95 Hypernode Pre-release, see https://github.com/bismuthfoundation/Bismuth-FAQ/blob/master/Hypernodes/HN_PreRelease_0-0-95.md


## Foreword
This is a draft trying to cover the main questions from the users about the Hypernodes.  
This does not replace detailed design and technical documents.


## Financial
Collateral

The minimum collateral amount to activate a Hypernode is 10 000 $BIS.  
The reward will be proportional to the collateral up to 30 000 $BIS, by increment of 10K $BIS.  
That is, instead of running 3 Hypernodes, you can run a single one with 30 000 $BIS collateral and get the same rewards.  
The upper limit is there so we don't end up with a single shared Hypernode :) 

We are not against shared Hypernodes but we want to keep the Hypernodes as decentralized as possible.


## Process
- The Hypernode itself is a python app to be run on a Linux host, along with a regular bismuth node.
- The Hypernode is activated by a controller wallet, whose private keys stay safe on your local computer.
- One wallet will be needed for each Hypernode and devoted to that task (separate from your usual wallet)
- Registration of a Hypernode is done by sending a specific transaction from the controller wallet (the one with the collateral)


## ROI
- For every block on Bismuth mainnet there will be a reward of 2.4IS for hypernodes
- Since there are 1440 blocks per day, that means a total reward for hypernodes of 3,456 $BIS per day
- The hypernode rewards per week are 24,192 $BIS, per month 103,680 $BIS and per year 1,261,440 $BIS.
- Running a hypernode requires a collateral amount of minimum 10,000 $BIS.
- Total HN collateral as well as current ROI can be found at https://hypernodes.bismuth.live/?page_id=163
- The ROI depends on the total amount of collateral. The more the HNs, the lower the ROI.

## Hosting

### Requirements

These are the minimal requirements, subject to modification. Final requirements may be less.  

- Linux host (vps or dedicated)
- 1 dedicated ipv4 address
- 40 GB SSD disk 
- 4GB Ram
- 2 CPU cores
- Unmetered bandwidth can be a plus.

> 1 CPU, 1Gb ram + 3Gb swap file has been tested as working, but may not in the future.  
You can give it a try, but that's not something we officially recommend.

> SSD disk is because the node does quite some I/Os, so having a fast disk makes a difference.


### Maintenance

Both the node and the Hypernode may need urgent updates.  
Even if the PoW chain does not need an hard fork, it may be necessary for Hypernode owners to update the Hypernode and/or the classic node.  
A dedicated info channel will be setup on discord, as well as tweets.

The update should be as simple as a git pull or could even be automated if you want to.

Failing to update in time means you may not get the rewards until you do.

### Hosting services

Many services are available to provide Hypernodes hosting.  
We are in contact with some of them and will tell as soon as something is available.  
If you are such a provider, feel free to contact us.

## Network Value

The goal of the Bismuth Hypernodes is to provide added value to the network.  
Some basic masternodes implementations are merely staking or check for a "ping" answer. How nice!

The Bismuth Hypernodes do operate on an entire different layer.  
They use a chain on their own, with no currency but metrics - KPIs - instead.  
Both chains are loosely coupled (see technical docs to read more) and operate in an almost independent manner, with very different rules.  
The Hypernodes operate on a Proof of Stake chain, with no mining and no competition between the Hypernodes.  
They observe and store their KPIs in the PoS chain.  
Since the Hypernodes are not part of the PoW chain, they do not add extra attack vectors.  

Eligible Hypernodes are derived from immutable info stored in the PoW chain.  
Quality index and bad behavior from both PoS and PoW nodes is recorded, immutable also, in the PoS chain for later action.

The independence of the two chains ensures:  
a/ It's way harder to manipulate the network, since it requires to forge both chains in very different ways.  
b/ bad actors and cheating attempts are recorded in an independent and immutable way. You can't cheat unnoticed.

Bismuth may be the first crypto currency to come with an integrated but independent KPIs dedicated side chain that monitors the network and ensures actors fair play.

## Governance

Since KPIs and levels will likely be used to trigger bans or actions against some actors, it's natural to ask for a governance process.  
At launch, given the experimental nature of the project however, this will not be possible.

The team will manually handle the analysis process, in order to have a good overview of the meaningful KPIs and their usage.

Later on, the filters could become autonomous and various parameters could be voted upon by Hypernodes owners.

