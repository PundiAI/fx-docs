# Snapshot Guide

## Types of f(x)Core Snapshots

1. A pruned-snapshot, which contains only the most recent day's data
   * Only data for the most recent day, no earlier data
   * Small amount of data and footprint
2. A snapshot of all the transaction data
   * Contains all transaction data, as well as current state data
   * Large amount of data and footprint
3. An archive-snapshot, which contains all transaction data as well as all state data (<mark style="color:red;">**syncing, not open yet**</mark>)
   * Contains all transaction and status data
   * The amount of data is very large and takes up a lot of space
   * In general, you do not need to use an archive node

## Available snapshots

<mark style="color:orange;">**Snapshots are performed based on its type as per below**</mark><mark style="color:orange;">,</mark> keeping a record of recent snapshot for as per below. If the date on the file is not yet updated and more than a week has lapsed since the last snapshot, you may replace the date in the file name to get the latest snapshot. <mark style="color:orange;">**If the date or day of the month are single digits, make sure to prepend a 0 in front of the single digit number. Date format will be in YYYY-MM-DD.**</mark>

{% tabs %}
{% tab title="Mainnet" %}
* Status data (pruned node)
  * Snapshot frequency: once a day (10am GMT +8)
  * Snapshot record keeping: 7 most recent
  * Shapshot link: [https://fx-mainnet.s3.amazonaws.com/fxcore-pruned-snapshot-mainnet-2022-08-01.tar.gz](https://fx-mainnet.s3.amazonaws.com/fxcore-pruned-snapshot-mainnet-2022-08-01.tar.gz)
  * Md5 link: [https://fx-mainnet.s3.amazonaws.com/fxcore-pruned-snapshot-mainnet-2022-08-01.tar.gz.md5](use-snapshot.md#types-of-f-x-core-snapshots)
* Data (full node)
  * Snapshot frequency: every Monday (10am GMT +8)
  * Snapshot record keeping: 3 most recent
  * Shapshot link: [https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2022-08-01.tar.gz](https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2022-08-01.tar.gz)
  * Md5 link: [https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2022-08-01.tar.gz.md5](https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2022-08-01.tar.gz.md5)
* Archive node - <mark style="color:red;">**synchronising**</mark>
  * Snapshot frequency: once a day (10am GMT +8)
  * Snapshot record keeping: 3 most recent
  * Shapshot link: [https://fx-mainnet.s3.amazonaws.com/fxcore-archive-snapshot-mainnet-2022-08-01.tar.gz](https://fx-mainnet.s3.amazonaws.com/fxcore-archive-snapshot-mainnet-2022-08-01.tar.gz)
  * Md5 link: [https://fx-mainnet.s3.amazonaws.com/fxcore-archive-snapshot-mainnet-2022-08-01.tar.gz.md5](https://fx-mainnet.s3.amazonaws.com/fxcore-archive-snapshot-mainnet-2022-08-01.tar.gz.md5)
{% endtab %}

{% tab title="Testnet" %}
* Status data (pruned node)
  * Snapshot frequency: once a day (10am GMT +8)
  * Snapshot record keeping: 7 most recent
  * Shapshot link: [https://fx-testnet.s3.amazonaws.com/fxcore-pruned-snapshot-testnet-2022-08-01.tar.gz](https://fx-testnet.s3.amazonaws.com/fxcore-pruned-snapshot-testnet-2022-08-01.tar.gz)
  * Md5 link: [https://fx-testnet.s3.amazonaws.com/fxcore-pruned-snapshot-testnet-2022-08-01.tar.gz.md5](https://fx-testnet.s3.amazonaws.com/fxcore-pruned-snapshot-testnet-2022-08-01.tar.gz.md5)
* Data (full node)
  * Snapshot frequency: every Monday (10am GMT +8)
  * Snapshot record keeping: 3 most recent
  * Shapshot link: [https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-testnet-2022-08-01.tar.gz](https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-testnet-2022-08-01.tar.gz)
  * Md5 link: [https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-testnet-2022-08-01.tar.gz.md5](https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-testnet-2022-08-01.tar.gz.md5)
* Archive node - <mark style="color:red;">**synchronising**</mark>
  * Snapshot frequency: once a day (10am GMT +8)
  * Snapshot record keeping: 3 most recent
  * Shapshot link: [https://fx-testnet.s3.amazonaws.com/fxcore-archive-snapshot-testnet-2022-08-01.tar.gz](https://fx-testnet.s3.amazonaws.com/fxcore-archive-snapshot-testnet-2022-08-01.tar.gz)
  * Md5 link: [https://fx-testnet.s3.amazonaws.com/fxcore-archive-snapshot-testnet-2022-08-01.tar.gz.md5](https://fx-testnet.s3.amazonaws.com/fxcore-archive-snapshot-testnet-2022-08-01.tar.gz.md5)
{% endtab %}
{% endtabs %}

## Downloading the Snapshots

First, you need to set your node up with the pre-requisites as per the node **setup guide**. Before you `start node` for fxcore to sync, follow the steps below to use snapshot.

> The greater the blockchain data, the more evident the reduction is syncing time will be. If the current state of the blockchain will take about 2 days to sync, this method of syncing will reduce the time to sync by at least 12 hours.

Download the Snapshot to your VM and decompress it to destination. To download and decompress the Snapshot Tar file to your VM you can run the following command:

{% tabs %}
{% tab title="Mainnet" %}
```
wget -c https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2022-08-01.tar.gz
```
{% endtab %}

{% tab title="Testnet" %}
```
wget -c https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-testnet-2022-08-01.tar.gz
```
{% endtab %}
{% endtabs %}

This will download the Snapshot of fxcore and decompress it in `~/.fxcore/` folder. Downloading the snapshot and unpacking the file will take some time.

{% hint style="info" %}
If the date or day of the month are single digits, make sure to prepend a 0 in front of the single digit number. Date format will be in YYYY-MM-DD. Sometimes

You need to ensure that you're running this command before you `Start` your node. If your fxcore node has already started, please stop it and then run the command below. Once unpacking is complete you can start the fxcore service again.
{% endhint %}

## MD5 checksum for downloaded file

Checksums are often used to verify data integrity but are not relied upon to verify data authenticity, below are example of the `md5sum` command to check whether the file is downloaded correctly using

```bash
$ md5sum fxcore-snapshot-mainnet-2022-08-01.tar.gz
4269fe416ca2d74d3925449f5ce7d214  fxcore-snapshot-mainnet-2022-08-01.tar.gz
```

> compare the md5 hash against https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2022-08-01.tar.gz.md

## Extracting the Snapshots

Now, to unpack the `tar` file in the fxcore Data directory run the following command:

{% tabs %}
{% tab title="Mainnet" %}
```
tar -xzvf fxcore-snapshot-mainnet-2022-08-01.tar.gz -C ~/.fxcore/
```
{% endtab %}

{% tab title="Testnet" %}
```
tar -xzvf fxcore-snapshot-testnet-2022-08-01.tar.gz -C ~/.fxcore/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Note that if your fxcore data directory is named differently then please rename that directory.

When you are unpacking the snapshot, it is already contained in a data folder, you will be replacing the data folder below. Be sure to maintain the integrity of this directory tree structure.
{% endhint %}

```
ubuntu@ip-192.168.0.100:~$ tree $HOME/.fxcore
/home/ubuntu/.fxcore
├── config
│   ├── app.toml
│   ├── client.toml
│   ├── config.toml
│   ├── genesis.json
│   ├── node_key.json
│   └── priv_validator_key.json
└── data
    └── priv_validator_state.json
2 directories, 6 files
```
