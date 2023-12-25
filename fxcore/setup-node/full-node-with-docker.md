# Full node with Docker

This guide will explain how to install the `fxcored mainnet` or `fxcored testnet` command line interface (CLI) on your system with `Docker` option. With these installed on a server, you can participate on the mainnet or testnet as a [Validator](../../validators/validator-setup.md).

## Install f(x)Core

> **You need to** [**install f(x)Core**](../installation.md) **before you go further**

### Use Docker

* Pull docker images

> if you do not already have docker installed, there will be a prompt for you to install it. Follow the instructions given.

{% tabs %}
{% tab title="Mainnet" %}
```
docker pull functionx/fx-core:5.0.0
```
{% endtab %}

{% tab title="Testnet" %}
```
docker pull functionx/fx-core:5.0.0
```
{% endtab %}
{% endtabs %}

* Initializing fxcore

{% tabs %}
{% tab title="Mainnet" %}
docker run --rm -v $HOME/.fxcore:/root/.fxcore functionx/fx-core:5.0.0 init fx-zakir --chain-id fxcore
{% endtab %}

{% tab title="Testnet" %}
docker run --rm -v $HOME/.fxcore:/root/.fxcore functionx/fx-core:5.0.0 init fx-zakir --chain-id dhobyghaut
{% endtab %}
{% endtabs %}

* Download genesis (copy and run each line, line by line)

{% tabs %}
{% tab title="Mainnet" %}
```
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v6.0.x/public/mainnet/genesis.json -O ~/.fxcore/config/genesis.json
```
{% endtab %}

{% tab title="Testnet" %}
{% code fullWidth="false" %}
```
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v6.0.x/public/testnet/genesis.json -O ~/.fxcore/config/genesis.json
```
{% endcode %}
{% endtab %}
{% endtabs %}

Upon startup the node will need to connect to peers. you can add peers to the `config.toml` config file:

{% tabs %}
{% tab title="Mainnet" %}
```bash
docker run --rm -v $HOME/.fxcore:/root/.fxcore functionx/fx-core:5.0.0 config config.toml p2p.seeds "c5877d9d243af1a504caf5b7f7a9c915b3ae94ae@fxcore-mainnet-seed-node-1.functionx.io:26656,b289311ece065c813287e3a25835bb6378999aa5@fxcore-mainnet-seed-node-2.functionx.io:26656,96f04dffc25ffcce11e179581d2a3ab6cb5535d5@fxcore-mainnet-node-1.functionx.io:26656,836ded83bac83a4ac8511826fa1ad4ca2238f960@fxcore-mainnet-node-2.functionx.io:26656,7c7a260eeefda37eac896ae423e78cf345a2ef70@fxcore-mainnet-node-3.functionx.io:26656,0fee38117655b6961319950d6beb929fb194217c@fxcore-mainnet-node-4.functionx.io:26656,6e8818051a2ca9b8be67a6f2ba48c33d8c489d5c@fxcore-mainnet-node-5.functionx.io:26656"
```
{% endtab %}

{% tab title="Testnet" %}
```bash
docker run --rm -v $HOME/.fxcore:/root/.fxcore functionx/fx-core:5.0.0 config config.toml p2p.seeds "e922b34e660976a64d6024bde495666752141992@dhobyghaut-seed-node-1.functionx.io:26656,a817685c010402703820be2b5a90d9e07bc5c2d3@dhobyghaut-node-1.functionx.io:26656,d22e741b4e8e2586dbe38fd348d3de8dfbb889a0@dhobyghaut-node-2.functionx.io:26656,c1a985c7e4c0b5ce6d343d87e070a63b24a76594@dhobyghaut-node-3.functionx.io:26656,cc267dac09a38b67b3bda0033f62678cb54bf843@dhobyghaut-node-4.functionx.io:26656,0ea7e81071d4004a1fbbe304477d8ca3183a5282@dhobyghaut-node-5.functionx.io:26656"
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
\*\* IMPORTANT At this stage \*\*BEFORE \*\*starting the node, please download the latest snapshot, refer to this [link](use-snapshot.md).

And at this stage, what is important is your validator keys that is stored in a json file for you to do a recovery in the future. For more [information](../../validators/validator-recovery.md) how to access the files.
{% endhint %}

* Run docker

```
docker run --name fxcore -d --restart=always -p 0.0.0.0:26656:26656 -p 127.0.0.1:26657:26657 -p 127.0.0.1:1317:1317 -p 127.0.0.1:26660:26660 -p 127.0.0.1:8545:8545 -p 127.0.0.1:8546:8546 -v $HOME/.fxcore:/root/.fxcore functionx/fx-core:5.0.0 start
```

To check if fxcore is synced:

```bash
curl localhost:26657/status
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

To ensure that the blocks are synced up with your node, under "sync\_info", "catching\_up value" should be false `"catching_up value": false`. This may take a few hours and your node has to be fully synced up before proceeding to the next step. You may cross reference the latest block you are synced to "sync\_info": "latest\_block\_height" and the latest block height of our Testnet blockchain on our [Testnet blockchain explorer](https://testnet-explorer.functionx.io/fxcore/blocks) or our [Mainnet](https://explorer.functionx.io/fxcore/proposals).

> Make sure that every node has a unique `priv_validator.json`. Do not copy the `priv_validator.json` from an old node to multiple new nodes. Running two nodes with the same `priv_validator.json` will cause you to double sign.
