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



## New config variables

### 

