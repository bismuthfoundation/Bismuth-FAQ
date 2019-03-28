# How to Backup your private Keys/wallet

The new tornado wallet uses one file for storing private keys, public keys and addresses, just like the legacy tkinter python wallet. It is named **wallet.json**. You just have to backup this file to be safe. However, there are some differences:


## Where is wallet.json stored?

The tornado based wallet tells you where it is storing your wallet.json  
That's also the path you have to copy your existing .der-files, if you want to import them.  

For instance the windows-path is:  
```
c:\users\"username"\bismuth-private
```

The Mac path is:
```
/Users/"username"/bismuth-private
```

If you start the wallet for the first time or without a wallet.json you should see this screen:

![Oups, where is the Screenshot?](graphics/new_start.png)

The **red circle 1** shows your local path, where your wallet.json is stored.  
If this is not the first time you start the wallet, you can get to this screen by clicking the wallet-looking button upper right (**red circle 2**).

Navigate to the local path and copy the wallet.json to a safe place.

**Important:**

If you haven't set a master password, the file is not encrypted and can be used by everyone, that has access to it.

However, if you set a master password, the file is fully encrypted and you also need to remember the master-password.  
Please also make a backup of the master password, if you think this is neccessary.  

**Loss of the master-password means loss of funds!**

We would advise to store a first version of the wallet.json - before encryption - in a safe and private place no one but you can access (physically secure usb key, safe box, encrypted offline storage you use for other things). This is a high value file, since access to it means being able to spend your funds.  
However you are then 100% sure you can access them even if you forget your master password later on (yes, this happens for real)

Once the wallet is encrypted with a master password, no info can leak without unlocking the wallet.  
- Your addresses are no more visible
- Your keys are encrypted
- The wallet file is totally unusable without the matching password.
- There is no hope of bruteforcing the password.
- If you lose your password and only have an encrypted wallet, no one - even the team - will be able to do anything for you.

Your keys, your BIS. Don't lose them.
