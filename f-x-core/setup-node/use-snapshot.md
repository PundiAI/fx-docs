# Snapshot Guide

## Available snapshots

### Snapshot files

```
# testnet
https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-testnet-2021-11-22.tar.gz

# mainnet
https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2021-11-22.tar.gz
```

> Snapshots are performed every Monday morning at 2:00 am UTC, keeping a record of that snapshot for three weeks. If the date on the file above is not yet updated and more than a week has lapsed since the last snapshot, you may replace the date (to the latest Monday's date) in the file name to get the latest snapshot. If the date or day of the month are single digits, make sure to prepend a 0 in front of the single digit number.

## Using Snapshots

First, you need to set your node up with the pre-requisites as per the node [setup guide](use-snapshot.md). Before you `start node` for fxcore to sync, follow the below steps to use snapshot.

> The greater the blockchain data, the more evident the reduction is syncing time will be. If the current state of the blockchain will take about 2 days to sync, this method of syncing will reduce the time to sync by at least 12 hours.

Download the Snapshot to your VM. To download the Snapshot Tar file to your VM you can run the following command:

```bash
wget -c <Snapshot URL>
```

For example:

```bash
# testnet
wget -c https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-testnet-2021-11-22.tar.gz

# mainnet
wget -c https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2021-11-22.tar.gz
```

This will download the Snapshot of fxcore. Downloading the snapshot and unpacking the file will take some time.

{% hint style="info" %}
You need to ensure that you’re running this command before you `Start` your node. If your fxcore node has already started, please stop it and then run the command below. Once unpacking is complete you can start the fxcore service again.
{% endhint %}

Now, to unpack the `tar` file in the fxcore Data directory run the following command:

```bash
tar -xzvf <snapshot file> -C <FXCORE_DATA_DIRECTORY>
```

For example:

```bash
# testnet
tar -xzvf fxcore-snapshot-testnet-2021-11-22.tar.gz -C ~/.fxcore/

# mainnet
tar -xzvf fxcore-snapshot-mainnet-2021-11-22.tar.gz -C ~/.fxcore/
```

{% hint style="info" %}
Note that if your fxcore data directory is named differently then please rename that directory.

When you are unpacking the snapshot, it is already contained in a data folder, you will be replacing the data folder below. Be sure to maintain the integrity of this directory tree structure.
{% endhint %}

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
