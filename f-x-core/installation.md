# Installation f(x)Core

This guide will explain how to install the `fxcored` entrypoint onto your system. With these installed on a server, you can participate in the mainnet as either a [Full Node](setup-node/) or a [Validator](../validators/validator-setup.md).

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

{% tabs %}
{% tab title="Ubuntu" %}
```
sudo apt-get update

sudo apt-get install -y make gcc
```

> Ps: `sudo apt-get install -y make gcc` may have encountered a problem with locked files, just try `sudo apt-get install -y make gcc` again.
{% endtab %}

{% tab title="Mac" %}
Ensure you have [Homebrew](https://brew.sh) installed.

Once you have Homebrew installed, you may run the following commands to install `make` and `gcc`:

```
brew install make
brew install gcc
```

We'll  be needing these commands later so let's install the necessary packages:

```
brew install git
brew install wget
```
{% endtab %}

{% tab title="Windows" %}
Ensure you have `make` and `gcc` installed and that the paths are set correctly for git bash.

One option for installing `gcc` can be found [here](https://jmeubank.github.io/tdm-gcc/articles/2021-05/10.3.0-release).

One option for installing `make` is using `chocolate` , more information can be found [here](https://chocolatey.org/install).

Once you have chocolate installed, run this command:

```
choco install make
```

Ensure you have all the necessary dependencies and compilers.

```
gcc --version
```

will return:

```
gcc.exe (tdm64-1) 10.3.0
Copyright (C) 2020 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE
```

and for make:

```
make --version
```

will return:

```
GNU Make 4.3
Built for Windows32
Copyright (C) 1988-2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```
{% endtab %}
{% endtabs %}

## Install Go

{% tabs %}
{% tab title="All other environments" %}
Install `go` by following the [official docs](https://golang.org/doc/install). Please select your respective environment❗

> For Ubuntu environment, there may be `permissions denied` issues with unzipping the go zip file, try using `sudo su` to resolve it.
{% endtab %}

{% tab title="If you are remoting into a terminal" %}
Especially if you are remoting into a Ubuntu terminal, run this command to download the `go` installer:

```
wget https://dl.google.com/go/go1.17.4.linux-amd64.tar.gz 
```

{% hint style="info" %}
After you have downloaded the package and you may proceed to step 2 of the [official docs](https://golang.org/doc/install). Choose your system OS and follow the instructions stated.
{% endhint %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Go 1.16+** or later is required for the f(x)Core. If you are remoting into a terminal, you may input the following command:
{% endhint %}

Setting environment variable:

```bash
mkdir -p $HOME/go/bin
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile
echo "export PATH=$PATH:$(go env GOPATH)/bin" >> ~/.profile
source ~/.profile
```

## Install the binaries

Next, let's install the latest version of f(x)Core. Make sure you have git installed if not you will be prompted to `install git`. Follow the instruction in the terminal.

{% tabs %}
{% tab title="All Other Environments" %}
```
git clone https://github.com/functionx/fx-core.git
cd fx-core
make go.sum

# testnet
make install-testnet

# mainnet
make install
```
{% endtab %}

{% tab title="Windows" %}
{% hint style="info" %}
You should run your commands in gitbash. But open fxcored.exe file using cmd prompt
{% endhint %}

```
git clone https://github.com/functionx/fx-core.git
cd fx-core

make go.sum

# mainnet
make build-win

#testnet
make build-win network=testnet

#use cmd prompt to open the fxcored.exe file
#in the path ./build/bin/fxcored.exe

#run the following commands to see if everything is ok:
./build/bin/fxcored.exe network
./build/bin/fxcored.exe version
```
{% endtab %}
{% endtabs %}



That will install the `fxcored` binary. Verfiy network:

```bash
fxcored network
```

The output should look something similar to this:

```
ChainId: dhobyghaut
CrossChainSupportBscBlock: "1"
CrossChainSupportPolygonBlock: "1"
CrossChainSupportTronBlock: "1"
GravityPruneValsetsAndAttestationBlock: "1"
GravityValsetSlashBlock: "1"
Network: testnet
```

{% hint style="info" %}
for Mainnet the ChainId should be **fxcore**
{% endhint %}

Verify version:

```bash
fxcored version --long

#or
fxcored version
```

`fxcored version --long` should output something similar to:

```bash
name: fxcore
server_name: fxcored
version: master-9b5596f54b9cadc001725a431fdfe22768c6e4c9
commit: 9b5596f54b9cadc001725a431fdfe22768c6e4c9
build_tags: netgo,ledger
go: go version go1.17.3 darwin/amd64
...
```

### Build Tags

Build tags indicate special features that have been enabled in the binary.

| Build Tag | Description                                     |
| --------- | ----------------------------------------------- |
| netgo     | Name resolution will use pure Go code           |
| ledger    | Ledger devices are supported (hardware wallets) |
