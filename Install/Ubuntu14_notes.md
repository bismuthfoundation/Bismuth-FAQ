# Install > 4.3.0 on Older Ubuntu 14 boxes

> This setup is not recommended. Ubuntu 18 is the reference setup.

These notes are here for reference only, to be used by advanced users knowing what they do.

## Python Version

Newest nodes require python3.6, ubuntu 14 comes with 3.5 stock.

Install ubuntu from ppa:

```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.6 python3.6-dev
```

Install matching pip:

`curl https://bootstrap.pypa.io/get-pip.py | sudo python3.6`

Install requirements

`pip3.6 install -r requirements-node.txt`


## Warning

- Never remove system python3.5
- Beware of default invocation (python3.6, not bare python nor python3)
- can play havoc with crons & all
