# BIS <> ETH Bridge

wBIS is an ERC-20 ETH Token with 1:1 native BIS counterpart.

## wBIS Token

- Chain: ETH Mainnet
- Contract address: `0xf5cB350b40726B5BcF170d12e162B6193b291B41`  ([View on etherscan](https://etherscan.io/token/0xf5cB350b40726B5BcF170d12e162B6193b291B41))
- Name: Wrapped BIS
- Decimals: 8 (same as native BIS)
- Custom logo: `https://raw.githubusercontent.com/bismuthfoundation/MEDIA-KIT/master/Logo_v2/wbis500x500.png`

The Eth bridge crystal comes with a one-click button to set up the correct custom token on your metamask extension. 

The Bridge is decentralized and runs thanks to a custom BIS protocol.  
It relies on Both a bridge and an oracle.  
- BIS Bridge Address: Bis1UBRiDGEQc9mBywXpwFZX6LF7hN4i8Qy9m ([View on explorer](http://bismuth.online/search?quicksearch=Bis1UBRiDGEQc9mBywXpwFZX6LF7hN4i8Qy9m))
- BIS Oracle Address: Bis1XETHbisxnShtghYQJDbf8o5gsQczW8Gp2 ([View on explorer](http://bismuth.online/search?quicksearch=Bis1XETHbisxnShtghYQJDbf8o5gsQczW8Gp2))

# Using the bridge

The bridge being decentralized, the user GUI is available as a Crystal within the Tornado Bismuth Wallet, since v0.1.41.  
[Tornado Bismuth Wallet releases](https://github.com/bismuthfoundation/TornadoWallet/releases)

Crystal menu, tick to activate "EthBridge", then restart the wallet (only needed for first activation).

## Pre-requisites

- A Tornado allet, version >= 0.1.41
- Activated EthBridge crystal, showing its logo
- Metamask installed with an ETH address configured and some ETH funds for tx fees  
- Basic knowledge of ETH and related gas fees


- Once the Ethbridge crystal is active, use the "Add to Metamask" green button from the EthBridge main menu to add the right token to your metamask.

## BIS to wBIS

> Send native BIS to the bridge, get wBIS on ETH (same amount less 5 BIS fees)

- Pick "BIS->WBIS 1/2" menu - also available from "swaps" page
- Fill in ETH recipient and amount
- Send the BIS
- Go to "Swaps" page
- wait for the BIS confs, then for the mint to be signed
- Use the "proxy mint" link to mint your wBIS (auto filled params)
- Your ETH wBIS balance is updated

[Detailled step by step with snapshots](BIS_TO_WBIS_DETAIL.md)

## wBIS to BIS

> Burn wBIS on ETH, get native BIS on BIS chain (same amount less 5 BIS fees)

- Pick "wBIS->BIS" menu - also available from "swaps" page
- Fill in BIS recipient and amount
- Check and adjust gas price
- Burn the wBIS
- wait for the ETH confs, then for the oracle and delivery to be signed
- You can go to "Swaps" page to monitor the progress
- Your BIS balance is updated


 
  
