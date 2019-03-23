# Quickstart with your new wallet

You just installed the new tornado wallet, here are some tipps how to start using it.

## First start

When you start the wallet, your default browser will open and show the following:

![Oups, where is the Screenshot?](graphics/first.png)

Let us notice some things. Left side is the navbar. A quick explanation of what we have there:

- Dashboard: Quick overview for balance and last transactions, news. Also send and receive are there.  
  Some Crystals will also show up on dashboard, if activated (for example bismuthprice)
- Transactions: Here, Balance is shown, send, receive and your last 10 transactions
- Messages: Here you can sign messages and in one of the next releases encrypt messages (Work in progress)
- Network: Here you can see detailed Infos like which server you are connected to as well as a list of available servers, network diff and block height
- Crystals: Addons that show additional info or add functionalities

Follow the links to get more details.

On top right of the main window, you can see the wallet-like sign with the lock.  
The lock shows your status, if you have a master password set (red for locked, transparent for unlocked like in the screenshot above)

![Oups, where is the Screenshot?](graphics/locked.png)

## manage addresses

On top left, you see "wallet info", click on the looking glass to manage your addresses.

![Oups, where is the Screenshot?](graphics/manage_addresses.png)

Here you can generate new addresses (up to 10 per wallet).  

You can generate labelled addresses or you can label addresses that don't have one yet.  
Click on the "pen"-button to edit labels.

If you click on an address or the "letter"-symbol under "Action" sets this address to **active-address**.  
The wallet will now show the corressponding balance or transactions to this address. So if you want to change through your addresses, this can be done here.

## Import addresses

many users will have existing private keys, that are stored in a .der-file format from the legacy wallet. It is possible to import them into the tornado-based wallet.

Copy your .der-files into the wallet directory, shown on top. It's the same directory as your wallet.json file.

After reloading the "manage addresses"-page, the wallet will show a list of files under **Potential legacy wallet imports**.

It shows the filename, the address it found in the file, encryption state and under "Action" the import button.

Clicking on "import" adds the address (and the corresponding keys) to the wallet.json file. So you do not have to import them every time you restart the wallet.

**Note:** The new wallet doesn't support encrypted legacy addresses yet.


## Dashboard

Once you selected an "active address", the Dashboard shows your balance, last transaction and you can send or receive BIS.

Remember, before you selected an active address, "Dashboard" showed you the screen to set a master password, or if you set one, that your wallet is locked and you need to unlock it.
