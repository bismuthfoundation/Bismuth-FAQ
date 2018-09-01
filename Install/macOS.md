# How to install the Bismuth Wallet on macOS

## Prerequisites

You need basic knowledge of your Mac and the Terminal app.

Python 3.6 or later is needed, as well as matching [pip](https://en.wikipedia.org/wiki/Pip_(package_manager)).

macOS currently still ships with Python 2.7, so you'll need to install Python 3.

We strongly recommend you to use [Homebrew](https://brew.sh) as described below, it's the best package manager for macOS and it'll make Python3 installation a breeze.
If you don't have Homebrew installed (`brew` command in the Terminal), please follow the easy instructions on [https://brew.sh](https://brew.sh) to install it.

### Install Python3

```
$ brew install python3   # this will also install pip3
```

macOS default Python 2.7 version remains available via the `python` and `pip` commands.

From now on, rather type `python3` and `pip3` to use the newly installed Python 3.

Manually install some dependencies just to make sure you have the latest versions:
```
$ pip3 install --upgrade simple-crypt
$ pip3 install --upgrade pillow
$ pip3 install --upgrade pycryptodomex
```

### Install Wget
macOS ships with `curl`, but most crypto guides out there rather use `wget`. It's useful to have `wget` around and with Homebrew it's super easy to install:
```
$ brew install wget
```

## Download and Install the wallet
Copy the latest **_Source code (tar.gz)_** URL from [https://github.com/hclivess/Bismuth/releases](https://github.com/hclivess/Bismuth/releases) (currently `4.2.6.2.tar.gz`) and paste it after the `wget` command as shown below:
```
$ cd     # go to Home folder. Use another path here if you don't want the wallet in your Home
$ wget https://github.com/hclivess/Bismuth/archive/4.2.6.2.tar.gz
```

Then:
```
$ tar -zxvf 4.2.6.2.tar.gz          # Extract the archive
$ mv Bismuth-4.2.6.2 Bismuth        # Rename "Bismuth-4.2.6.2" folder to "Bismuth"
$ cd Bismuth                        # Enter "Bismuth" folder
$ pip3 install -r requirements.txt  # Install wallet dependencies
```

Note: with the light wallet you don't need a Bismuth node anymore, nor a complete chain sync to run and use the wallet.


## Run the wallet

```
$ python3 wallet.py
```

This will create a new `wallet.der` file in the _Bismtuh_ folder.
This is your wallet, **backup that file in a safe place**.
If you already have a wallet, place your `wallet.der` in the _Bismtuh_ folder before running the wallet.
