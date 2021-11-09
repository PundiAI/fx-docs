# Snapshot Guide

## Available snapshots

### Testnet

```
https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-2021-11-08.tar.gz
```

### Mainnet

```
...
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
wget -c https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-2021-11-08.tar.gz
```

This will download the Snapshot of fxcore

{% hint style="info" %}
You need to ensure that you’re running this command before you `Start` your node. If your fxcore node has already started, please stop it and then run the command below. Once unpacking is complete you can start the fxcore service again.
{% endhint %}

Now, to unpack the `tar` file in the fxcore Data directory run the following command:

```bash
tar -xzvf <snapshot file> -C <FXCORE_DATA_DIRECTORY>
```

For example:

```bash
tar -xzvf fxcore-snapshot-2021-11-08.tar.gz -C ~/.fxcore/data/
```

Note that if your fxcore data directory is named differently then please rename that directory.
