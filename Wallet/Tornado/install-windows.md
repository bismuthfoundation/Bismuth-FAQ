# Install on Windows

### Install with Win-Installer:

Get it here: https://github.com/bismuthfoundation/TornadoWallet/releases

Download the "TornadoBismuthWalletInstaller.exe" and install it. Install procedure is tested under Win10. Older versions may show problems like this: "program can't start because api-ms-win-crt-runtime-l1-1-0.dll is missing".
Here is a possible solution: https://www.microsoft.com/en-US/download/details.aspx?id=48145
Eventually you get a warning from Win Defender. If you downloaded the installer from the upper link, it is as safe, as github as a platform can be. 

After successful installation, go to the folder where you installed the wallet (standard path should be C:\Program Files (x86)\TornadoBismuthWallet) and start it with the **TornadoBismuthWallet.exe**.

If you doubleclicked the file, a terminal gets opened -  you can ignore **BUT NOT CLOSE it!** There you see some Info for debugging and statistics and what the wallet does under the hood.
It also opens a browser window: http://127.0.0.1:8888/wallet/info and here you go with your local language (if available, otherwise it should be english).

![Oups, where is the Screenshot?](graphics/tornado.png)

***
### Install it from source with pip:
Under windows you can also run the Tornado wallet from the source-files. You need python installed (preferably 3.7, minimum is 3.6) and then do:

clone it with a git tool or download a zip-file from (https://github.com/bismuthfoundation/TornadoWallet) , click the green "clone or download" - as zip. Extract it on your local pc, open a cmdline-window and navigate to the folder c:\your\selected\path\TornadoWallet and then type: 

`python -m pip install -r requirements.txt`

if you have not used a Bismuth-Wallet before, you need also pycryptodome:
See https://pycryptodome.readthedocs.io/en/latest/src/installation.html#windows-from-sources-python-3-5-and-newer for help.
Follow the 3 steps on their documentation page.

if everything is installed, change dir to wallet and there start the TornadoWallet with:

`python wallet.py`

![Oups, here should be a CLI-Screenshot](graphics/cli.png)

***

### Options for wallet start

You can also start the wallet with some additional options:

![Oups, here should be a CLI-Screenshot](graphics/cli_verbose.png)

Use --verbose if you want to see more CLI output, for example if something shows a weird behaviour, so you can add the message to a bug report.



![Oups, here should be a CLI-Screenshot](graphics/cli_lang.png)

If you do not want to start the wallet with your systems standard language, you can add --lang="language abbreviation". This will start the wallet with the selected language. It will start with english, if you enter an unknown abbreviation. You can still switch the language after the wallet has started.



It is also possible to start the wallet with --debug to get lots of information on the CLI. IT is not recommended for normal users, but it also does no harm to the function itself.