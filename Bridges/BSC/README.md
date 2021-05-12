# BIS <> BSC Bridge

wBIS is an BEP-20 BNB Token with 1:1 native BIS counterpart.

## wBIS Token

- Chain: BSC Smart Chain
- Contract address: `0x56672ecb506301b1E32ED28552797037c54D36A9`  ([View on bscscan](https://bscscan.com/token/0x56672ecb506301b1E32ED28552797037c54D36A9))
- Name: Wrapped BIS
- Decimals: 8 (same as native BIS)
- Custom logo: `https://raw.githubusercontent.com/bismuthfoundation/MEDIA-KIT/master/Logo_v2/wbis500x500.png`

<img src="wbis500x500t.png" width="100" height="100" />

The Bsc bridge crystal comes with a one-click button to set up the correct custom token on your metamask extension. 

The Bridge is decentralized and runs thanks to a custom BIS protocol.  
It relies on Both a bridge and an oracle.  
- BIS Bridge Address: Bis1FWBNBbrYZkQmCYQf8FQWTUCFEreUeT71 ([View on explorer](http://bismuth.online/search?quicksearch=Bis1FWBNBbrYZkQmCYQf8FQWTUCFEreUeT71))
- BIS Oracle Address: Bis1XBNBY66wngPLVwbRbnSs4ScVBwE3iM7kL ([View on explorer](http://bismuth.online/search?quicksearch=Bis1XBNBY66wngPLVwbRbnSs4ScVBwE3iM7kL))

# Using the bridge

The bridge being decentralized, the user GUI is available as a Crystal within the Tornado Bismuth Wallet, since v0.1.43.  
[Tornado Bismuth Wallet releases](https://github.com/bismuthfoundation/TornadoWallet/releases)

Crystal menu, tick to activate "BscBridge", then restart the wallet (only needed for first activation).

## Pre-requisites

- A Tornado allet, version >= 0.1.43
- Activated BscBridge crystal, showing its logo
- Metamask installed with an BSC address configured and some BNB funds for tx fees  
- Basic knowledge of BSC and related gas fees


- Once the Bscbridge crystal is active, use the "Add to Metamask" green button from the BscBridge main menu to add the right token to your metamask.

## BIS to wBIS

> Send native BIS to the bridge, get wBIS on BSC (same amount less 5 BIS fees)

- Pick "BIS->WBIS 1/2" menu - also available from "swaps" page
- Fill in BSC recipient and amount **double check your BSC address, no going back**
- Send the BIS
- Go to "Swaps" page
- wait for the BIS confs, then for the mint to be signed
- Use the "proxy mint" link to mint your wBIS (auto filled params)
- Your BNB wBIS balance is updated

[Detailled step by step with screenshots](BIS_TO_WBIS_DETAIL.md)

## wBIS to BIS

> Burn wBIS on BSC, get native BIS on BIS chain (same amount less 5 BIS fees)

- Pick "wBIS->BIS" menu - also available from "swaps" page
- Fill in BIS recipient and amount  **double check your BIS address, no going back**
- Check and adjust gas price
- Burn the wBIS
- wait for the BSC confs, then for the oracle and delivery to be signed
- You can go to "Swaps" page to monitor the progress
- Your BIS balance is updated

# Liquidity Farming

WIP
 
