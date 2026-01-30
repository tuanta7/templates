# Development Environment Setup

## Go

```shell
# go to home directory
cd ~

# install Go
wget https://go.dev/dl/go1.25.0.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.25.0.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc
go version

# back to the previous directory
cd -
```

## NVM + NodeJS

```shell
# install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion

source ~/.bashrc
nvm --version

# install node
nvm install --lts
```

## Minikube

Local single-node Kubernetes for development/testing.

```

```
