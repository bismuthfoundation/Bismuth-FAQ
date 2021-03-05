# Bismuth Keys and safety

Bismuth is a brand new thing. What you think you know with BTC or such is not necessarly true here :)

## Where is my wallet?

The wallet is the ledger (blockchain). It's everywhere, on every BIS node.
You can't "save" your wallet.

## How is my wallet secured?

Your keys are all that is needed to access your wallet.  
They are the important thing to care about.

## So, Where are my keys?
**This depends on the Bismuth wallet you are using**

- [Lightwallet](#Lightwallet)
- [TK-Wallet](#TK-Wallet)
- [Tornado Bismuth Wallet](#Tornado-Bismuth-Wallet)
- [My Bismuth Wallet (Android Mobile)](#My-Bismuth-Wallet)
- [Paper Wallet](#Paper-Wallet)

## Lightwallet
**Bismuth Light Wallet Version: before 4.2.4.1**

When you run a node for the first time, the node generates a brand new address for you, and a set of private/public key.  
Those are the 3 files in your bismuth dir:
- address.txt
- pubkey.der
- privkey.der

Both .der files you have to take care of. Backup in a secure place, and do not, EVER, give you privkey to anyone.

**Bismuth Light Wallet - Since Version 4.2.4.1**

When you run a node for the first time, there are no keys present, the node generates a brand new address for you and one file containing the private and the public key:
- wallet.der
you only need to backup this file and you are safe. **Don't ever give this file to anyone!**

If you upgrade from an older wallet and if you have a pubkey.der and privkey.der, the node automatically converts the keys to the new format:
- if your upgrade is done, FIRST copy the pubkey.der and privkey.der into your bismuth-folder.
- Start the node.
- your wallet.der was generated, backup it.

If you already started the node before you copied your old keys:
- if you have no funds on the newly generated address, you can delete wallet.der (you can keep it by renaming the file wallet.der (for example wallet_address.der)
- if you already have funds on this new address, rename the file wallet.der (for example wallet_address.der)
- copy your old privkey.der and pubkey.der into the bismuth-folder
- start the node
- the node converts your old keys to wallet.der

Since version 4.2.4.1, you can't use "load wallet" with privkey.der and pubkey.der. You need to convert all your addresses to the new format. After this step, you can use "load Wallet" like before.

**Since Version 4.2.7 (Bismuth_installer_nuitka.exe)**

Since version 4.2.7 there where changes to the location of wallet.der, your wallet can be found in C:\Program Files (x86)\Bismuth\files,  these changes had to be made because of Anti Virus issues with the previous " Bismuth_installer.exe".

## TK-Wallet
TK-Wallet - https://github.com/bismuthfoundation/tk-wallet 

The TK-Wallet supersedes the Lightwallet.  
- RSA address scheme  
- wallet file: wallet.der
- version 0.9.2 creates automatically a wallet.json file on startup, can be used with Tornado Wallet
- option to backup the wallet within the wallet (it creates a tar.gz file which which contains 2 files, wallet.der and wallet.json):  
Wallet > Backup Wallet  

## Tornado-Bismuth-Wallet 
Tornado Bismuth Wallet - https://github.com/bismuthfoundation/TornadoWallet 

- RSA address scheme
- multi-address wallet
- wallet opens in the Browser: http://127.0.0.1:8888/
- wallet file: wallet.json
- wallet file directory is shown at this URL: http://127.0.0.1:8888/wallet/load

## My-Bismuth-Wallet
My Bismuth Wallet - https://github.com/redDwarf03/my_bismuth_wallet/

- ECDSA address scheme
- Mobile Wallet (Android)
- HD Wallet
- Seed - Store the 24 words to be able to regenerate the key pairs
- Paper Wallet compatibility

## And how do I protect them?

Your privkey is only really needed to spent from your account.  
So, if you run a node, use one set of keys (with no BIS) to run your node, then a second set for your wallet, from another dir.

## Cold wallet?

If you want some kind of cold storage, and do not need to spent your BIS right now, you can:  
- run a node on an airgaped machine
- copy the address.txt, you'll need it to send BIS to.
- store on secure offline storage the .der files and matching address.txt
- delete the .der
- generate Keys offline using the HD Paper Wallet

Then, you'll be able to check your balance at any moment, via http://bismuth.online for instance.  
You will be able to feed your account at any time, just need your address

But you will need to copy your privkey.der to an online machine to spent from it.

## why isn't my pub key also my address?

Told ya, bismuth ain't btc.


## What about Encrypt/Password my wallet?

To encrypt you wallet.der, open wallet.exe or wallet.py and click on "send" tab and you will see "encrypt" click on that and enter a new password and thats will be your wallet encrypted.

**if you back up your wallet.der before you encrypt your wallet.der please note your back-up will not have a password on it, to be sure your wallet.der back-up has a password on it,  back it up after your have encrypted**

## Paper-Wallet

BIS Paper - https://github.com/AngainorDev/BIS-Paper
- HD Paper wallet
- Paper Wallet seed can be imported into the mobile 'My Bismuth Wallet'
- ECDSA keys scheme
- Support for Chameleon wallet seeds (After september 2019), see "options" button
