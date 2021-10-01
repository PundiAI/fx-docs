This guide will explain how to install the `fxcored testnet` or `fxcored master` entrypoint onto your system.
With these installed on a server, you can participate in the mainnet or testnet as a [Validator](../validators/validator-setup.md).

# Install f(x)Core

> **You need to [install f(x)Core](./installation.md) before you go further**

### Setup f(x)Core

Initializing fxcore：
```bash
fxcored init fx-zakir
```

Fetching config file (copy this entire line of code):
```bash
# testnet
wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/genesis.json -O ~/.fxcore/config/genesis.json && \
wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/config.toml -O ~/.fxcore/config/config.toml && \
wget https://raw.githubusercontent.com/functionx/fx-core/testnet/public/app.toml -O ~/.fxcore/config/app.toml

# mainnet
...
```

Start Node:
```bash
nohup fxcored start 2>&1 > fxcore.log
```

View more startup configurations:
```bash
nohup fxcored start 2>&1 > fxcore.log & fxcored start -h
```

For example, Start and open the 1317 restful service port:
```bash
nohup fxcored start --api.enable true  --address 0.0.0.0:1317  2>&1 > fxcore.log & tail -f fxcore.log
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
To ensure that the blocks are synced up with your node, under "sync_info", "catching_up value" should be false
`"catching_up value": false`.
This may take a few hours and your node has to be fully synced up before proceeding to the next step.
You may cross reference the latest block you are synced to "sync_info": "latest_block_height" and the latest block height of our Testnet blockchain on our [Testnet blockchain explorer](https://aabbcc-explorer.functionx.io/fxcore/blocks).

Stop Node (will be running in the background if not stopped):
```bash
ps -ef | grep fxcored
kill -9 <PID>
```