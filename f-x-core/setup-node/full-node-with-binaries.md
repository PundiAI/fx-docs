# Full node with Binaries

This guide will explain how to install the `fxcored mainnet` or `fxcored testnet` command line interface (CLI) on your system. With these installed on a server, you can participate on the mainnet or testnet as a [Validator](../../validators/validator-setup.md).

## Install f(x)Core

> **You need to** [**install f(x)Core**](../installation.md) **before you go further**

#### Setup f(x)Core

Initializing fxcore:

```bash
fxcored init <your_name>
```

Initializing fxcored will result in the creation of a few directories and most importantly the .fxcore directory (for more information on the directory tree, refer to the validator-recovery section). This will be where your validator keys are stored and this is important for recovery of your validator.

Fetching config file (copy this entire line of code and hit <mark style="color:red;">ENTER</mark>):

{% tabs %}
{% tab title="Mainnet" %}
```
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.1.x/public/mainnet/genesis.json -O ~/.fxcore/config/genesis.json
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.1.x/public/mainnet/config.toml -O ~/.fxcore/config/config.toml
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.1.x/public/mainnet/app.toml -O ~/.fxcore/config/app.toml
```
{% endtab %}

{% tab title="Testnet" %}
```
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.1.x/public/testnet/genesis.json -O ~/.fxcore/config/genesis.json
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.1.x/public/testnet/config.toml -O ~/.fxcore/config/config.toml
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.1.x/public/testnet/app.toml -O ~/.fxcore/config/app.toml
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
\*\* IMPORTANT At this stage \*\*BEFORE \*\*starting the node, please download the latest snapshot, refer to this [link](use-snapshot.md).

Additionally, what is important is that your validator keys that is stored in a .json file for you to do a recovery in the future. For more [information](../../validators/validator-recovery.md) how to access the files.

Also, you can consider generating a new consensus key and [backing it up using a pin](full-node-with-binaries.md#secret-and-updating-consensus-key).
{% endhint %}

{% hint style="info" %}
For a more stable way of setting up via [running a daemon](full-node-with-binaries.md#running-server-as-a-daemon)
{% endhint %}

Start Node:

```bash
nohup fxcored start 2>&1 > fxcore.log &
```

Check logs:

```bash
tail -f fxcore.log
```

View more startup configurations:

```bash
fxcored start -h
```

For example, Start and open the 1317 restful service port:

```bash
nohup fxcored start --api.enable true --address 0.0.0.0:1317 2>&1 > fxcore.log &
```

The execution of the previous command will return something like this (this is to check the status of nodes and which blocks are being synced/are syncing):

```bash
2 height=12893 module=state num_txs=0
5:05PM INF minted coins from module account amount=21043599215998775405FX from=mint module=x/bank
5:05PM INF indexed block height=12893 module=txindex
5:05PM INF executed block height=12894 module=state num_invalid_txs=0 num_valid_txs=0
5:05PM INF commit synced commit=436F6D6D697449447B5B32382032362035392036332037352031313920323920343520313230203137203137203630203438203133322032352031303220313734203134392038362039203139203138332031383520323332203136382039312031323420313234203220313320313138203137305D3A333235457D
5:05PM INF committed state app_hash=1C1A3B3F4B771D2D7811113C30841966AE95560913B7B9E8A85B7C7C020D76AA height=12894 module=state num_txs=0
5:05PM INF indexed block height=12894 module=txindex
5:05PM INF minted coins from module account amount=21043602938137090338FX from=mint module=x/bank
5:05PM INF executed block height=12895 module=state num_invalid_txs=0 num_valid_txs=0
5:05PM INF commit synced commit=436F6D6D697449447B5B32343320323039203634203234332031393220343120313238203138302034352031363720313038203135382031363520313430203338203233302032203136372032303720333320313535203138322031373020313738203234322035392031333220362031323720393920313132203132305D3A333235467D
```

To check if fxcore is synced:

```bash
curl localhost:26657/status
or
fxcored status
```

Return:

```bash
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "node_info": {
      "protocol_version": {
        "p2p": "8",
        "block": "11",
        "app": "0"
      },
      "id": "123868554adafd679f5dc6367bddea39aa5adb94",
      "listen_addr": "tcp://0.0.0.0:26656",
      "network": "fxcore",
      "version": "v0.34.9",
      "channels": "40202122233038606100",
      "moniker": "moniker",
      "other": {
        "tx_index": "on",
        "rpc_address": "tcp://0.0.0.0:26657"
      }
    },
    "sync_info": {
      "latest_block_hash": "239609FE5FD475389C1ACFCEF46DCF6B0343F0C04E43A7968677809C2D489F3F",
      "latest_app_hash": "0D2F1299950E0DE86BFF1CDEEEDE3BA57F7899EF1492A6E6809DF3060164046D",
      "latest_block_height": "810805",
      "latest_block_time": "2021-09-01T07:56:29.166926257Z",
      "earliest_block_hash": "12B0FB286BD34C077CACF97D3D2757B27C49E63FB81E6262399FF11A3C3C002E",
      "earliest_app_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "earliest_block_height": "1",
      "earliest_block_time": "2021-07-12T07:42:23.292429Z",
      "catching_up": false
    },
    "validator_info": {
      "address": "214999E9412502DE8DE13F626F9D32D41C1B5015",
      "pub_key": {
        "type": "tendermint/PubKeyEd25519",
        "value": "fUlPjsYSLi04vwzeNiFObBVabV6TKhB6WB3c65iEayU="
      },
      "voting_power": "0"
    }
  }
```

To ensure that the blocks are synced up with your node, under "sync\_info", "catching\_up value" should be false `"catching_up value": false`. This may take a few hours and your node has to be fully synced up before proceeding to the next step. You may cross reference the latest block you are synced to "sync\_info": "latest\_block\_height" and the latest block height of our Testnet blockchain on our [Testnet blockchain explorer](https://dhobyghaut-explorer.functionx.io/fxcore/blocks) or our [Mainnet](https://explorer.functionx.io/fxcore/proposals).

Stop Node (will be running in the background if not stopped):

```bash
ps -ef | grep fxcored
kill -9 <PID>
```

## Running Server as a Daemon

It is important to keep `fxcored` running at all times. There are several ways to achieve this, and the simplest solution we recommend is to register `fxcored` as a `systemd` service so that it will automatically get started upon system reboots and other events.

### Register `fxcored` as a service

First, create a service definition file in `/etc/systemd/system`.

Run this command to create the sample file above in the file path`/etc/systemd/system/fxcored.service` (if you are in the fx-core directory):

```
cat > /etc/systemd/system/fxcored.service
```

hit the <mark style="color:red;background-color:blue;">ENTER</mark> button on your keyboard and `copy` and `paste` the contents of the file below into the command line:

#### Sample file:

{% code title="fxcored.service" %}
```
[Unit]
Description=f(x)Core Node
After=network.target
[Service]
Type=simple
User=root
WorkingDirectory=/root
ExecStart=/root/go/bin/fxcored start --home /root/.fxcore
Restart=on-failure
RestartSec=3
LimitNOFILE=4096
[Install]
WantedBy=multi-user.target
```
{% endcode %}

Then hit the <mark style="color:red;background-color:blue;">ENTER</mark> button on your keyboard before using <mark style="color:red;background-color:blue;">Ctrl+D</mark> on your keyboard, your file with the above contents will be created. It should look like this:

{% hint style="info" %}
run the command:`which fxcored` and replace the ExecStart file path with the return value of the command
{% endhint %}

```
root@XXXXXXXXXXXXXXX:~# cat > /etc/systemd/system/fxcored.service
[Unit]
Description=f(x)Core Node
After=network.target
[Service]
Type=simple
User=root
WorkingDirectory=/root
ExecStart=/root/go/bin/fxcored start --home /root/.fxcore
Restart=on-failure
RestartSec=3
LimitNOFILE=4096
[Install]
WantedBy=multi-user.target
```

Modify the `Service` section from the given sample above to suit your settings. Note that even if we raised the number of open files for a process, we still need to include `LimitNOFILE`.

After creating a service definition file, you should execute:

```
systemctl daemon-reload
systemctl enable fxcored
```

### Controlling the service

Use `systemctl` to control (start, stop, restart)

{% tabs %}
{% tab title="Start" %}
```
sudo systemctl start fxcored
```
{% endtab %}

{% tab title="Stop" %}
```
sudo systemctl stop fxcored
```
{% endtab %}

{% tab title="Restart" %}
```
sudo systemctl restart fxcored
```
{% endtab %}

{% tab title="Status" %}
```
sudo systemctl status fxcored
```
{% endtab %}
{% endtabs %}

To start the node, run `sudo systemctl start fxcored`, and thereafter run `journalctl -t fxcored -f` to see the latest and continuous logs.

### Accessing logs

{% tabs %}
{% tab title="Entire Log" %}
```
journalctl -t fxcored
```
{% endtab %}

{% tab title="Entire log reversed" %}
```
journalctl -t fxcored -r
```
{% endtab %}

{% tab title="Latest and continuous log" %}
```
journalctl -t fxcored -f
```
{% endtab %}
{% endtabs %}

> Concluding tips: It is always better to sync f(x)Core using the Daemon method because this ensures stability and that your syncing is continuously running in the background.

## Secret and updating consensus key

Use this at your own risk❗ I suggest trying it out on testnet first and also backing up your `priv_validator_key.json` if you already have this set up for a validator. The file can be found in this file path `/.fxcore/config/priv_validator_key.json`.

### Updating your consensus key and tagging it to a pin

Before setting up your validator if you would like to have a backup of your keys with a pin. You may run the following command:

```
fxcored tendermint unsafe-reset-priv-validator <secret>
```

{% hint style="info" %}
the \<secret> key here must be longer than 32 characters
{% endhint %}

Running the above command will return:

```
WARNING: The consensus private key of the node will be replaced.
Ensure that the backup is complete.
USE AT YOUR OWN RISK. Continue? [y/N]
```

After inputting `y`, your `priv_validator_key.json` will be replaced with a new file. This means you will have a new consensus private key and it will be tagged to your `<secret>` pin.

### Checking to see if the pin works

If your .fxcore folder is in the root directory

```
cat ~/.fxcore/config/priv_validator_key.json
```

Record the previous output before running the next command to remove this file:

```
rm ~/.fxcore/config/priv_validator_key.json
```

The following command will recover your original consensus key:

```
fxcored tendermint unsafe-reset-priv-validator <secret>
```

Match this output with the previous output above:

```
cat ~/.fxcore/config/priv_validator_key.json
```
