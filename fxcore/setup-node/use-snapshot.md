# Snapshot Guide

{% hint style="warning" %}
<mark style="color:yellow;">**WARNING**</mark>

1. First, you need to setup your node up with the pre-requisites as per the node **setup guide**
2. <mark style="color:red;">**Before you start**</mark> your node for fxcore to sync, follow the steps below to use snapshot
3. If your fxcore node has already started, <mark style="color:red;">**please stop it**</mark> and then follow the steps below to use the snapshot
4. Once unpacking is complete, you can start the fxcore service again
{% endhint %}

## Types of f(x)Core Snapshots

1. A pruned-snapshot, which contains only the most recent day's data
   * Only data for the most recent day, no earlier data
   * Small amount of data and footprint
2. A full-node snapshot, which contains all the transaction data
   * Contains all transaction data, as well as current state data
   * Large amount of data and footprint

> The greater the blockchain data, the more evident the reduction is syncing time will be. If the current state of the blockchain will take about 2 days to sync, this method of syncing will reduce the time to sync by at least 12 hours.

## Available snapshots

<mark style="color:orange;">**Snapshots are performed based on its type as per below**</mark><mark style="color:orange;">,</mark> keeping a record of recent snapshot for as per below. If the date on the file is not yet updated and more than a week has lapsed since the last snapshot, you may replace the date in the file name to get the latest snapshot. <mark style="color:orange;">**If the date or day of the month are single digits, make sure to prepend a 0 in front of the single digit number. Date format will be in YYYY-MM-DD.**</mark>

{% tabs %}
{% tab title="Mainnet" %}
* Pruned-shapshot (light node)
  * Snapshot frequency: once a day (10am GMT +8)
  * Snapshot record keeping: 3 most recent
  * Shapshot link: [https://fx-mainnet.s3.amazonaws.com/fxcore-pruned-snapshot-mainnet-2024-12-30.tar.gz](https://fx-mainnet.s3.amazonaws.com/fxcore-pruned-snapshot-mainnet-2024-12-30.tar.gz)
  * Md5 link: [https://fx-mainnet.s3.amazonaws.com/fxcore-pruned-snapshot-mainnet-2024-12-30.tar.gz.md5](https://fx-mainnet.s3.amazonaws.com/fxcore-pruned-snapshot-mainnet-2024-12-30.tar.gz.md5)
* Full-node snapshot (full node)
  * Snapshot frequency: every Monday (12am GMT +8)
  * Snapshot record keeping: 1 most recent
  * Shapshot link: [https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2024-12-30.tar.gz](https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2024-12-30.tar.gz)
  * Md5 link: [https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2024-12-30.tar.gz.md5](https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2024-12-30.tar.gz.md5)
{% endtab %}

{% tab title="Testnet" %}
* Pruned-shapshot (light node)
  * Snapshot frequency: once a day (10am GMT +8)
  * Snapshot record keeping: 3 most recent
  * Shapshot link: [https://testnet-fx.s3.amazonaws.com/fxcore-pruned-snapshot-testnet-2024-12-30.tar.gz](https://testnet-fx.s3.amazonaws.com/fxcore-pruned-snapshot-testnet-2024-12-30.tar.gz)
  * Md5 link: [https://testnet-fx.s3.amazonaws.com/fxcore-pruned-snapshot-testnet-2024-12-30.tar.gz.md5](https://testnet-fx.s3.amazonaws.com/fxcore-pruned-snapshot-testnet-2024-12-30.tar.gz.md5)
* Full-node snapshot (full node)
  * Snapshot frequency: every Monday (10pm GMT +8)
  * Snapshot record keeping: 1 most recent
  * Shapshot link: [https://testnet-fx.s3.amazonaws.com/fxcore-snapshot-testnet-2024-12-30.tar.gz](https://testnet-fx.s3.amazonaws.com/fxcore-snapshot-testnet-2024-12-30.tar.gz)
  * Md5 link: [https://testnet-fx.s3.amazonaws.com/fxcore-snapshot-testnet-2024-12-30.tar.gz.md5](https://testnet-fx.s3.amazonaws.com/fxcore-snapshot-testnet-2024-12-30.tar.gz.md5)
{% endtab %}
{% endtabs %}

## Downloading the Snapshots

Download the snapshot to your VM. To download the snapshot tar file to your VM you can run the following command:

{% tabs %}
{% tab title="Mainnet" %}
```
wget -c https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2024-12-30.tar.gz
```
{% endtab %}

{% tab title="Testnet" %}
```
wget -c https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-testnet-2024-12-30.tar.gz
```
{% endtab %}
{% endtabs %}

This will download the snapshot of fxcore full-node data. Since it's full-node, downloading the snapshot and unpacking the file will take some time.

## MD5 checksum for downloaded file

Checksums are often used to verify data integrity but are not relied upon to verify data authenticity, below are example of the `md5sum` command to check whether the file is downloaded correctly using

```bash
$ md5sum fxcore-snapshot-mainnet-2024-12-30.tar.gz
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  fxcore-snapshot-mainnet-2024-12-30.tar.gz
```

> compare the md5 hash against https://fx-mainnet.s3.amazonaws.com/fxcore-snapshot-mainnet-2024-12-30.tar.gz.md5

## Extracting the Snapshots

Now, to unpack the `tar` file in the fxcore Data directory run the following command:

{% tabs %}
{% tab title="Mainnet" %}
```
rm -rf ~/.fxcore/data
tar -xzvf fxcore-snapshot-mainnet-2024-12-30.tar.gz -C ~/.fxcore/
```
{% endtab %}

{% tab title="Testnet" %}
```
rm -rf ~/.fxcore/data
tar -xzvf fxcore-snapshot-testnet-2024-12-30.tar.gz -C ~/.fxcore/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Note that if your fxcore data directory is placed on different directory then please rename to that

When you are unpacking the snapshot, it is already contained in a data folder, you will be replacing the data folder below. Be sure to maintain the integrity of this directory tree structure.
{% endhint %}

```
root@host:~# ls -l ~/.fxcore/data/
total 64
drwxr-xr-x 2 root root 36864 Aug  2 16:49 application.db
drwxr-xr-x 2 root root  4096 Aug  2 16:26 blockstore.db
drwx------ 2 root root  4096 Aug  2 16:44 cs.wal
drwxr-xr-x 2 root root  4096 Aug  2 15:32 evidence.db
-rw------- 1 root root    46 Aug  1 10:02 priv_validator_state.json
drwxr-xr-x 3 root root  4096 Aug  1 10:02 snapshots
drwxr-xr-x 2 root root  4096 Aug  2 16:48 state.db
drwxr-xr-x 2 root root  4096 Aug  2 16:29 tx_index.db
```
