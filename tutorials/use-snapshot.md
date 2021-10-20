# Snapshots Guide

## Available snapshots

### testnet

```
https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-2021-10-11.tar.gz
```

### mainnet

```
...
```

## Using Snapshots

First, you need to set up your node with pre-requisites as per the node [setup guide](../join-mainnet.md). Before you start services for fxcore to sync, follow the below steps to use snapshot:

Download the Snapshot to your VM. To download the Snapshot Tar file to your VM you can run the following command

```bash
wget -c <Snapshot URL>
```

For example:

```bash
wget -c https://.../fxcore-snapshot-2021-10-11.tar.gz
```

This will download the Snapshot of fxcore.

Now, to unpack the Tar file in the fxcore Data directory run the following command. You need to ensure that you’re running this command before you Start fxcore service on your node. If your fxcore service has started, please stop and then run the below command. Once unpacking is complete you can start the fxcore service again.

```bash
tar -xzvf <snapshot file> -C <FXCORE_DATA_DIRECTORY>
```

For example:

```bash
tar -xzvf fxcore-snapshot-2021-10-11.tar.gz -C ~/.fxcore/data/
```

Note that if your fxcore data directory is different then please mention that directory name correctly.
