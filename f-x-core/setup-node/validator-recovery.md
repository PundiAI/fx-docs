# Validator Recovery

Run the following command to open the `priv_validator_key.json` file and store it somewhere for safe keeping:

```
# if you are in fx-core dir
cat ../.fxcore/config/priv_validator_key.json
```

return (this will be your private key to recovery your validator):

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

The directory tree of the .fxcore directory should look like this:

```
ubuntu@ip-192.168.0.100:~$ tree $HOME/.fxcore
/home/ubuntu/.fxcore
├── config
│   ├── app.toml
│   ├── config.toml
│   ├── genesis.json
│   ├── node_key.json
│   └── priv_validator_key.json
└── data
    └── priv_validator_state.json
2 directories, 6 files
```

The command after `Initializing fxcore` from setting up node with [Full node with Binaries](full-node-with-binaries.md) or [Full node with Docker ](full-node-with-docker.md)is to override the various files that were initialized earlier:

```
# Binaries:
wget https://raw.githubusercontent.com/functionx/fx-core/master/public/testnet/genesis.json -O ~/.fxcore/config/genesis.json
wget https://raw.githubusercontent.com/functionx/fx-core/master/public/testnet/config.toml -O ~/.fxcore/config/config.toml
wget https://raw.githubusercontent.com/functionx/fx-core/master/public/testnet/app.toml -O ~/.fxcore/config/app.toml

# Docker:
sudo wget https://raw.githubusercontent.com/functionx/fx-core/master/public/testnet/genesis.json -O ~/.fxcore/config/genesis.json
sudo wget https://raw.githubusercontent.com/functionx/fx-core/master/public/testnet/config.toml -O ~/.fxcore/config/config.toml
sudo wget https://raw.githubusercontent.com/functionx/fx-core/master/public/testnet/app.toml -O ~/.fxcore/config/app.toml
```

The key file here is `priv_validator_key.json`. After initializing and overriding those files, override the_ _`priv_validator_key.json` with your original `priv_validator_key.json` of the validator you want to recover. You may do this by following the command below (if you are in .fxcore/config directory):

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
