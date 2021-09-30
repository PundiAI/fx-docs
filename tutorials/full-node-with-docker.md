This guide will explain how to install the `fxcored testnet` or `fxcored master` entrypoint onto your system.
With these installed on a server, you can participate in the mainnet or testnet as a [Validator](../validators/validator-setup.md).


# Install f(x)Core

> **You need to [install f(x)Core](./installation.md) before you go further**

## Use Docker

* pull docker images
> if you do not already have docker installed, there will be a prompt for you to install it. Follow the instructions given.

```bash
# For testnet：
  docker pull functionx/fx-core:testnet
# For mainnet：
  ...  
```

* init config

```bash
# For testnet：
  docker run --rm -v ~:/root functionx/fx-core:testnet init fx-zakir
# For mainnet：
  ...  
```

* download genesis

```bash
# For testnet：
sudo wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/genesis.json -O ~/.fxcore/config/genesis.json
sudo wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/config.toml -O ~/.fxcore/config/config.toml
sudo wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/app.toml -O ~/.fxcore/config/app.toml
# For mainnet：  
  ...
```

* run docker

```bash
# For testnet：
  docker run --name fxcore -d -p 26656:26656 -p 26657:26657 -p 1317:1317 -p 26660:26660 -v ~:/root functionx/fx-core:testnet start
# For mainnet：
  ... 
```

* Check the status of nodes：
To check if fxcore is synced
On the remote machine/VM, run `curl localhost:26657/status`
In the output, catching_up value should be false
This may take a few hours and your node has to be fully synced up before proceeding to the next step.

## Upgrading Your Node

These instructions are for full nodes that have ran on previous versions of and would like to upgrade to the latest testnet.

### Reset Data

First, remove the outdated files and reset the data.

```bash
rm $HOME/.fxcore/config/addrbook.json $HOME/.fxcore/config/genesis.json
fxcored unsafe-reset-all
```

Your node is now in a pristine state while keeping the original `priv_validator.json` and `config.toml`.
If you had any sentry nodes or full nodes setup before,
your node will still try to connect to them, but may fail if they haven't also been upgraded.


> Make sure that every node has a unique `priv_validator.json`.
Do not copy the `priv_validator.json` from an old node to multiple new nodes.
Running two nodes with the same `priv_validator.json` will cause you to double sign.
