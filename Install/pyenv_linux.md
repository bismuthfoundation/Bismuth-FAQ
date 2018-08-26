# Using pyenv on linux to install another python

Pyenv allow to install as many python versions as you need without breaking the system default python version.

## Prerequisite

```sudo apt-get install -y git make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev```

## Install pyenv_installer

See https://github.com/pyenv/pyenv-installer :

`curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash`

edit `~/.bashrc` and add these lines:  
```
export PATH="/root/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"```

`source ~/.bashrc`

## Install a Python Version

`pyenv install 3.6.4`

make this version global by default:  
`pyenv global 3.6.4`

reinstall modules  
```pip install pysocks pycryptodomex requests```
