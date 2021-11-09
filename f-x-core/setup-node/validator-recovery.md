# Validator Recovery

After initializing fxcore:

```
# Binaries
fxcored init fx-zakir

# Docker
docker run --rm -v ~:/root functionx/fx-core:testnet init fx-zakir
```

Run the following command:

```
#if you are in fx-core dir
cd ../.fxcore

# if you are in the root dir
cd .fxcore
```

Running the command:

```
ls
```

You will see a .fxcore directory in your root folder with the following directory tree:

![](<../../.gitbook/assets/Fxcore Private KEy.jpg>)

To open the `priv_validator_key.json` file and store it somewhere for safe keeping:

```
cd config
cat priv_validator_key.json
```

The command after is to override the various files that were initialized earlier:

```
# Binaries:
wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/genesis.json -O ~/.fxcore/config/genesis.json && \
wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/config.toml -O ~/.fxcore/config/config.toml && \
wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/app.toml -O ~/.fxcore/config/app.toml

# Docker:
sudo wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/genesis.json -O ~/.fxcore/config/genesis.json
sudo wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/config.toml -O ~/.fxcore/config/config.toml
sudo wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/app.toml -O ~/.fxcore/config/app.toml
```

The key file here is `priv_validator_key.json`_. _After initializing and overriding those files, override the `priv_validator_key.json`_ _with your original `priv_validator_key.json` of the validator you want to recover. You may do this by following the command below (if you are in .fxcore/config directory):

```
cat > priv_validator_key.json
```

Hit the <mark style="color:red;background-color:blue;">ENTER</mark> button on your keyboard and `copy` and `paste` the contents of your `priv_validator_key.json` file from your original validator into the command line

Your command line should look something like this:

```
root@XXXXXXXXXXXXXXX:~# cat > priv_validator_key.json
{
  "address": "XXXXXXXXXXXXXXXXXXxxxxxxxXXXXXX",
  "pub_key": {
    "type": "tendermint/PubKeyEd25519",
    "value": "XXXXXXXXXXXXXXXXXXxxxxxxxXXXXXX"
  },
  "priv_key": {
    "type": "tendermint/PrivKeyEd25519",
    "value": "XXXXXXXXXXXXXXXXXXxxxxxxxXXXXXX"
  }}
```

Then use <mark style="color:red;background-color:blue;">Ctrl+D</mark> on your keyboard, your file with the above contents will be created.

Run the following command and compare if the public key you generate now matches the old public key. If it does, then you have successfully recovered your original validator.

```
fxcored tendermint show-validator
```
