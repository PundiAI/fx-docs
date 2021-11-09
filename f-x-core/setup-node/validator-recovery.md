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
cd ..
cd .fxcore

# if you are in the root dir
cd .fxcore
```

&#x20;You will see a .fxcore directory in your root folder with the following directory tree:

![](<../../.gitbook/assets/Fxcore Private KEy.jpg>)

To open the priv\_validator\_key.json file and store it somewhere for safe keeping:

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

The key file here is priv\_validator_\__key.json. After initializing and overriding those files, override the priv\_validator_\__key.json with your original priv\_validator_\__key.json of the validator you want to recover.

Your priv\_validator_\__key.json file should look something like this:

```
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

Run the following command and compare if the public key you generate now matches the old public key. If it does, then you have successfully recovered your original validator.

```
fxcored tendermint show-validator
```
