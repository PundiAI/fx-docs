---
description: >-
  The documentation corresponding contains details for the RPC - HTTP, WS and
  GRPC endpoints
---

# f(x)Core Network

#### f(x)Core supports different clients in order to support Cosmos and Ethereum/Web3 transactions and queries

| <p><br></p>                                                       | Description                                                                  | Default Port |
| ----------------------------------------------------------------- | ---------------------------------------------------------------------------- | ------------ |
| **Cosmos gRPC**                                                   | Query or send f(x)Core transactions using gRPC                               | `9090`       |
| **Cosmos restAPI**                                                | Query or send f(x)Core transactions using restAPI                            | `1317`       |
| **Web3** [**JSON-RPC**](web3/)                                    | Query Web3-formatted transactions and blocks or send Web3 txs using JSON-RPC | `8545`       |
| **Web3 Websocket**                                                | Subscribe to Web3 logs and events emitted in smart contracts.                | `8546`       |
| **Tendermint** [**RPC**](json-rpc-api/)                           | Subscribe to f(x)Core logs and events emitted in smart contracts.            | `26657`      |
| **Tendermint Websocket**                                          | Query transactions, blocks, consensus state, broadcast transactions, etc.    | `26657`      |
| **Command Line Interface** ([**CLI**](../fxcore/installation.md)) | Query or send f(x)Core transactions using your Terminal or Console.          | N/A          |

#### Various clients, tools and end points available

{% hint style="info" %}
<mark style="color:yellow;">For adding custom network to MetaMask/Defi Wallet, please use the</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">`Web3 JSON RPC`</mark>
{% endhint %}

{% tabs %}
{% tab title="f(x)Core Mainnet" %}
|                         |                                                                                                           |
| ----------------------- | --------------------------------------------------------------------------------------------------------- |
| ChainId                 | `fxcore`                                                                                                  |
| Native Coin             | `FX`                                                                                                      |
| Block Explorer          | [https://starscan.io](https://starscan.io)                                                                |
| Tm JSON RPC             | <p><code>https://fx-json.functionx.io:26657</code><br>or<br><code>https://fx-json.functionx.io</code></p> |
| Tm JSON RPC Websocket   | `wss://fx-json.functionx.io:26657/websocket`                                                              |
| gRPC                    | `https://fx-grpc.functionx.io:9090`                                                                       |
| REST API                | `https://fx-rest.functionx.io:1317`                                                                       |
| EVM ChainID             | 530                                                                                                       |
| EVM Block Explorer      | [https://starscan.io/evm](https://starscan.io/evm)                                                        |
| Web3 JSON RPC           | https://fx-json-web3.functionx.io:8545                                                                    |
| Web3 JSON RPC Websocket | wss://fx-json-web3.functionx.io:8546                                                                      |
{% endtab %}

{% tab title="f(x)Core Testnet" %}
|                         |                                                                              |
| ----------------------- | ---------------------------------------------------------------------------- |
| ChainId                 | `dhobyghaut`                                                                 |
| Native Coin             | `FX`                                                                         |
| Block Explorer          | [https://testnet.starscan.io](https://testnet.starscan.io)                   |
| Tm JSON RPC             | https://testnet-fx-json.functionx.io:26657                                   |
| Tm JSON RPC Websocket   | wss://testnet-fx-json.functionx.io:26657/websocket                           |
| gRPC                    | https://testnet-fx-grpc.functionx.io:9090                                    |
| Rest                    | https://testnet-fx-rest.functionx.io:1317                                    |
| EVM ChainID             | 90001                                                                        |
| EVM Block Explorer      | [https://testnet.starscan.io/evm](https://testnet.starscan.io/evm)           |
| Web3 JSON RPC           | https://testnet-fx-json-web3.functionx.io:8545                               |
| Web3 JSON RPC Websocket | wss://testnet-fx-json-web3.functionx.io:8546                                 |
| Testnet Faucet          | [https://testnet-faucet.functionx.io/](https://testnet-faucet.functionx.io/) |
{% endtab %}
{% endtabs %}
