# Installation f(x)Core

This guide will explain how to install the `fxcored` entrypoint onto your system. With these installed on a server, you can participate in the mainnet as either a Full Node or a [Validator](../validators/validator-setup.md).

## Hardware Requirements

We recommend the following for running f(x)Core:

* 2 or more CPU cores
* At least 500G of disk storage
* At least 4G of memory
* At least 10mbps network bandwidth

To see a [quick cloud setup](../resources/cloud-setup.md) on how to setup and deploy it on the cloud.

## Install build requirements

Install `make` and `gcc`.

On Ubuntu this can be done with the following commands:

```bash
sudo apt-get update

sudo apt-get install -y make gcc
```

Ps: `sudo apt-get install -y make gcc` may have encountered a problem with locked files, just try `sudo apt-get install -y make gcc` again.

## Install Go

Install `go` by following the [official docs](https://golang.org/doc/install). There may be `permissions denied` issues with unzipping the go zip file, try using `sudo su` to resolve it.

> **Go 1.16+** or later is required for the f(x)Core. If you are remoting into a terminal, you may input the following command:

```bash
wget https://dl.google.com/go/go1.17.3.linux-amd64.tar.gz 
```

After you have downloaded the package and you may proceed to step 2 of the [official docs](https://golang.org/doc/install). Choose your system OS and follow the instructions stated.

Setting environment variable:

```bash
mkdir -p $HOME/go/bin
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile
echo "export PATH=$PATH:$(go env GOPATH)/bin" >> ~/.profile
source ~/.profile
```

## Install the binaries

Next, let's install the latest version of f(x)Core. Make sure you have git installed if not you will be prompted to `install git`. Follow the instruction in the terminal.

```bash
git clone https://github.com/functionx/fx-core.git
cd fx-core
make go.sum

# testnet
make install-testnet

# mainnet
make install
```

That will install the `fxcored` binary. Verify that everything is OK:

```bash
fxcored version --long
```

`fxcored` for instance should output something similar to:

```bash
name: fxcore
server_name: fxcored
version: github-b0b51a2312903309336826c73c8fd3b5fc81cdbb
commit: b0b51a2312903309336826c73c8fd3b5fc81cdbb
build_tags: netgo,ledger
go: go version go1.16.2 darwin/amd64
...
```

### Build Tags

Build tags indicate special features that have been enabled in the binary.

| Build Tag | Description                                     |
| --------- | ----------------------------------------------- |
| netgo     | Name resolution will use pure Go code           |
| ledger    | Ledger devices are supported (hardware wallets) |
