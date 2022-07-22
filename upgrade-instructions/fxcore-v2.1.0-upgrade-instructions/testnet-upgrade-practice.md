---
description: >-
  This doc will help validators (re)experience the node upgrade process of
  'fxv2' version on the existing testnet
---

# Testnet Upgrade Practice

Because the current testnet upgrade has already been completed, we would have to use an older snapshot data and the previous version of f(x)Core to simulate the upgrade.

We will first download the snapshot data that occurred before the testnet upgrade. Since the testnet upgrade happened at 7:00pm on June 23, we will:

1. Ensure you have set up the necessary [requirements](testnet-upgrade-practice.md#requirements) and [environmental variables](testnet-upgrade-practice.md#modify-your-paths)
2. [Install fxcore before the testnet upgrade](testnet-upgrade-practice.md#undefined)
3. Download the [snapshot](testnet-upgrade-practice.md#undefined) at 6:50pm on June 23
4. Then [start the testnet node](testnet-upgrade-practice.md#5.-start-your-node-with-the-fxcore-v1-binaries)
5. Wait for the testnet node to [synchronise to the upgrade height](testnet-upgrade-practice.md#undefined) (estimated time to catch up is \~10 minutes)
6. Finally [replace the testnet node with the 'fxv2' version](testnet-upgrade-practice.md#7.)

#### Requirements

Follow the [installation instructions](https://hub.cosmos.network/main/getting-started/installation.html) to understand build requirements. You'll need to install **Go 1.18+**.

```shell
sudo apt update
sudo apt upgrade
sudo apt install git build-essential

curl -OL https://golang.org/dl/go1.18.1.linux-amd64.tar.gz
sudo tar -C /usr/local -xvf go1.18.1.linux-amd64.tar.gz
```

#### Modify your paths

```shell
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.profile
source ~/.profile
```

#### 1.  Install fxcore before testnet upgrade

```shell
cd $HOME
git clone https://github.com/functionx/fx-core.git
cd fx-core
git checkout testnet/v1.2.x
go mod tidy
FX_BUILD_OPTIONS=testnet make build
./build/bin/fxcored version
```

#### 2.  Initialise fxcore configuration

```shell
cd $HOME/fx-core
./build/bin/fxcored init --chain-id=dhobyghaut upgradev2 --home="$HOME/testnet/fxcore"
```

#### 3.  Download the testnet configuration file

```shell
wget https://raw.githubusercontent.com/functionx/fx-core/testnet/v1.2.x/public/testnet/genesis.json -O ~/testnet/fxcore/config/genesis.json
wget https://raw.githubusercontent.com/functionx/fx-core/testnet/v1.2.x/public/testnet/config.toml -O ~/testnet/fxcore/config/config.toml
wget https://raw.githubusercontent.com/FunctionX/fx-core/testnet/v1.2.x/public/testnet/app.toml -O ~/testnet/fxcore/config/app.toml
```

#### 4.  Download the snapshot before the testnet upgrade and unzip it

```bash
wget -c https://fx-testnet.s3.amazonaws.com/fxcore-snapshot-testnet-2022-06-23.tar.gz

tar -xzvf fxcore-snapshot-testnet-2022-06-23.tar.gz -C ~/testnet/fxcore
```

#### 5. Start your node with the fxCore v1 binaries

```bash
cd $HOME/fx-core
./build/bin/fxcored start --home ~/testnet/fxcore
```

When the node synchronises to the upgrade height (blk 3418880), the node will stop consensus

When you see the following log information, it means that the node has stopped participating in consensus:

```bash
.....

3:44AM INF committed state app_hash=046E66E9810ECB7DB85A9F17296582B83F03701F71564CC693E721ED2FCD61FF height=3418877 module=state num_txs=1 server=node
3:44AM INF minted coins from module account amount=31066496235061858712FX from=mint module=x/bank
3:44AM INF indexed block height=3418877 module=txindex server=node
3:44AM INF executed block height=3418878 module=state num_invalid_txs=0 num_valid_txs=0 server=node
3:44AM INF commit synced commit=436F6D6D697449447B5B31373020313820313239203636203230342031383520353320343020323434203532203234322031343920302039302031333720343920313138203138362039312031303220323530203136382037203230322032362031393720313438203230352031333820313638203134342034375D3A3334324146457D
3:44AM INF committed state app_hash=AA128142CCB93528F434F295005A893176BA5B66FAA807CA1AC594CD8AA8902F height=3418878 module=state num_txs=0 server=node
3:44AM INF minted coins from module account amount=31066498286443317365FX from=mint module=x/bank
3:44AM INF indexed block height=3418878 module=txindex server=node
3:44AM INF executed block height=3418879 module=state num_invalid_txs=0 num_valid_txs=0 server=node
3:44AM INF commit synced commit=436F6D6D697449447B5B313138203134362032303720313134203439203137342032343920323133203020333120363520313536203232342031312031363720323430203137392032313820313332203136382031363020323132203131312032312032323320313537203231392032372032333720313731203131372031335D3A3334324146467D
3:44AM INF committed state app_hash=7692CF7231AEF9D5001F419CE00BA7F0B3DA84A8A0D46F15DF9DDB1BEDAB750D height=3418879 module=state num_txs=0 server=node
3:44AM ERR UPGRADE "fxv2" NEEDED at height: 3418880: {"binaries":{"linux/amd64":"https://github.com/FunctionX/fx-core/releases/download/v2.0.0-rc2/fxcored-v2.0.0-rc2-linux-amd64?checksum=sha256:0454ab6ca8939ca6a12b9738613dd2b9e13e1c34f5813dee49eee9482e3a0acd","darwin/amd64":"https://github.com/FunctionX/fx-core/releases/download/v2.0.0-rc2/fxcored-v2.0.0-rc2-darwin-amd64?checksum=sha256:c22560ac82de4cc5bef6516d8e3d1067eaefae62a2383c6da167da13bffc9e5d","windows/amd64":"https://github.com/FunctionX/fx-core/releases/download/v2.0.0-rc2/fxcored-v2.0.0-rc2-windows-amd64.exe?checksum=sha256:43d292706cadab7715b95cd6dd429ab4d8adea05e5b5dcfa624c2c0add731393"}}
panic: UPGRADE "fxv2" NEEDED at height: 3418880: {"binaries":{"linux/amd64":"https://github.com/FunctionX/fx-core/releases/download/v2.0.0-rc2/fxcored-v2.0.0-rc2-linux-amd64?checksum=sha256:0454ab6ca8939ca6a12b9738613dd2b9e13e1c34f5813dee49eee9482e3a0acd","darwin/amd64":"https://github.com/FunctionX/fx-core/releases/download/v2.0.0-rc2/fxcored-v2.0.0-rc2-darwin-amd64?checksum=sha256:c22560ac82de4cc5bef6516d8e3d1067eaefae62a2383c6da167da13bffc9e5d","windows/amd64":"https://github.com/FunctionX/fx-core/releases/download/v2.0.0-rc2/fxcored-v2.0.0-rc2-windows-amd64.exe?checksum=sha256:43d292706cadab7715b95cd6dd429ab4d8adea05e5b5dcfa624c2c0add731393"}}

goroutine 156 [running]:
github.com/cosmos/cosmos-sdk/x/upgrade.BeginBlocker({{_, _}, _, {_, _}, {_, _}, _}, {{0x279aba0, 0xc000130020}, ...}, ...)
        github.com/cosmos/cosmos-sdk@v0.42.11/x/upgrade/abci.go:70 +0xee8
github.com/cosmos/cosmos-sdk/x/upgrade.AppModule.BeginBlock(...)
        github.com/cosmos/cosmos-sdk@v0.42.11/x/upgrade/module.go:127
github.com/cosmos/cosmos-sdk/types/module.(*Manager).BeginBlock(_, {{0x279aba0, 0xc000130020}, {0x27a8c68, 0xc00cbf7880}, {{0xb, 0x0}, {0xc01f2de824, 0xa}, 0x342b00, ...}, ...}, ...)
        github.com/cosmos/cosmos-sdk@v0.42.11/types/module/module.go:330 +0x3a2
github.com/functionx/fx-core/app.(*App).BeginBlocker(_, {{0x279aba0, 0xc000130020}, {0x27a8c68, 0xc00cbf7880}, {{0xb, 0x0}, {0xc01f2de824, 0xa}, 0x342b00, ...}, ...}, ...)
        github.com/functionx/fx-core/app/app.go:681 +0xdb
github.com/cosmos/cosmos-sdk/baseapp.(*BaseApp).BeginBlock(_, {{0xc0081167e0, 0x20, 0x20}, {{0xb, 0x0}, {0xc01f2de824, 0xa}, 0x342b00, {0x3ae25747, ...}, ...}, ...})
        github.com/cosmos/cosmos-sdk@v0.42.11/baseapp/abci.go:192 +0x97c
github.com/tendermint/tendermint/abci/client.(*localClient).BeginBlockSync(_, {{0xc0081167e0, 0x20, 0x20}, {{0xb, 0x0}, {0xc01f2de824, 0xa}, 0x342b00, {0x3ae25747, ...}, ...}, ...})
        github.com/tendermint/tendermint@v0.34.19/abci/client/local_client.go:280 +0x118
github.com/tendermint/tendermint/proxy.(*appConnConsensus).BeginBlockSync(_, {{0xc0081167e0, 0x20, 0x20}, {{0xb, 0x0}, {0xc01f2de824, 0xa}, 0x342b00, {0x3ae25747, ...}, ...}, ...})
        github.com/tendermint/tendermint@v0.34.19/proxy/app_conn.go:81 +0x55
github.com/tendermint/tendermint/state.execBlockOnProxyApp({0x279bce8?, 0xc006f29260}, {0x27a1a20, 0xc0054dbb30}, 0xc00ac8f4a0, {0x27a9ab0, 0xc0054dadd0}, 0x342aff?)
        github.com/tendermint/tendermint@v0.34.19/state/execution.go:307 +0x3dd
github.com/tendermint/tendermint/state.(*BlockExecutor).ApplyBlock(_, {{{0xb, 0x0}, {0xc006a77a74, 0x7}}, {0xc006a77a90, 0xa}, 0x1, 0x342aff, {{0xc0250f7900, ...}, ...}, ...}, ...)
        github.com/tendermint/tendermint@v0.34.19/state/execution.go:140 +0x171
github.com/tendermint/tendermint/blockchain/v0.(*BlockchainReactor).poolRoutine(0xc000d01340, 0x0)
        github.com/tendermint/tendermint@v0.34.19/blockchain/v0/reactor.go:398 +0xb5a
created by github.com/tendermint/tendermint/blockchain/v0.(*BlockchainReactor).OnStart
        github.com/tendermint/tendermint@v0.34.19/blockchain/v0/reactor.go:110 +0x7a
```

#### 6.  Stop node manually

```bash
Ctrl/Control+C
```

#### 7.  Restart the node with fxCore v2.1.1

```bash
git checkout release/v2.1.x
go mod tidy
make build
./build/bin/fxcored version
./build/bin/fxcored start --home ~/testnet/fxcore
```

You will then see the node migration and upgrade log. The entire process which will take \~10 minutes. Once you see the node starting to synchronise blocks, it means that the node upgrade has been successful.

```bash
.....

4:02AM INF applying upgrade "fxv2" at height: 3418880
4:02AM INF clear kv store chainId=dhobyghaut
4:02AM INF clear kv store storesName=feemarket
4:02AM INF clear kv store done consumeMs=7780 storesName=feemarket
4:02AM INF clear kv store storesName=evm
4:02AM INF clear kv store done consumeMs=6431 storesName=evm
4:02AM INF clear kv store storesName=erc20
4:02AM INF clear kv store done consumeMs=908136 storesName=erc20
4:02AM INF clear kv store storesName=migrate
4:02AM INF clear kv store done consumeMs=2589502 storesName=migrate
4:02AM INF update FX metadata metadata="description:\"The native staking token of the Function X\" denom_units:<denom:\"FX\" > base:\"FX\" display:\"FX\" name:\"Function X\" symbol:\"FX\" "
4:02AM INF update block params chainId=dhobyghaut
4:02AM INF update block params before update="max_bytes:1048576 max_gas:-1 "
4:02AM INF update block params after update="max_bytes:1048576 max_gas:30000000 "
4:02AM INF start to run module v2 migrations...
4:02AM INF adding a new module: authz
4:02AM INF migrating module bank from version 1 to version 2
4:02AM INF migrating module distribution from version 1 to version 2
4:02AM INF adding a new module: feegrant
4:02AM INF migrating module gov from version 1 to version 2
4:02AM INF migrating module ibc from version 1 to version 2
4:02AM INF migrating module slashing from version 1 to version 2

.....
```
