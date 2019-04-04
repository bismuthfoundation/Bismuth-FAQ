# CLI-Server start options

You just installed the new tornado-based Bismuth wallet - from CLI with 

```
python3 wallet.py (example using ubuntu)
```

This will open your Browser, connecting to 127.0.0.1:8888. It is the standard value, but there are some more options.

## Connect to a Tornado Server

In fact, starting the "wallet" means, you are starting a tornado based server, that accepts connections to present a GUI - the wallet. You can change the port it listens on, if you want to use another:

```
python3 wallet.py --port=1234
```

If you already have a service running on port 8888, this is the option to use, to solve the conflict.

Maybe more interesting is the fact, that you are able to connect to the tornado server also from other machines in your network. Let us assume, the machine running the tornado server has 192.168.1.2 as ip-address

```
python3 wallet.py --listen=192.168.1.0
```

This would allow every machine in the same subnet to connect to your tornado server. So you could run one server at home and connect from every device. However, remember, that keys are handled locally. Using the wallet from any device means to have the private key also on any device.

you could also do the following, but **this is not recommended**

python3 wallet.py --listen=0.0.0.0

This would allow every device, even from outside your network, to connect to your tornado server. Normally private users are behind a router that is running as a NAT-device. But this should be considered as absolutely unsafe. Allow only what is really neccessary and if you only want to use it on the same device, start the wallet with defaults.



## Defining CLI output level

If you start the wallet without any option, the cli output is at the minimum. If you want to see more, what is going on under the hood, or you are facing an issue and want to analyze it further (or give the devs more detailed info), you can start the tornado server with

```
python3 wallet.py --verbose
```

If you really want to (but not recommended for normal usage), you can have maximum output with

```
python3 wallet.py --debug
```



## Connecting to a specific wallet server

If you want to connect to a specific wallet server (maybe because, you are running your own, or the one, the wallet autoconnects to is having issues), you can achieve that with:

```
python3 wallet.py --server="ipv4-address":8150
```

If you enter a wrong address here (or the server does not accept any more clients), the wallet is using the API-info and tries to connect to another wallet server.



## Force a language of your choice

On startup, the wallet tries to identify the language of your OS and then use the same if there is a translation ready. However, if don't want to use it, there is the possibility to select the language on startup (you can also change it on the GUI with few clicks)

```
python3 wallet.py --lang=ru
```

you need to enter the abbreviation, suiting for your language. If you are unsure, all that are supported can be find here: https://github.com/bismuthfoundation/TornadoWallet/tree/master/wallet/locale. If you use a non exisiting one, the wallet will start in english.



## Themes

As you may know, the wallet supports themes, the actual release only supports the "material"-theme, which is default on startup. In later releases (or if you want to develop a theme yourself), you can select the theme, the wallet should start with

```
python3 wallet.py --theme="themes/material"
```

The themes are stored in folder *themes*. So if you want to use another, or add themes later on (maybe from third party source), you have to copy them into this folder.



## (de)activate crystals

Crystals are the name for addons in the tornado-based wallet. The actual integrated crystals reach from Bismuth Exchange-Price information, Mining-Pool info to an integration for the 2 existing Bismuth-powered games dragginator and autogame.

On default, the wallet starts with crystals activated. If you want to start without them for whatever reason, you can achieve this with following option

```
python3 wallet.py --crystals=False
```

