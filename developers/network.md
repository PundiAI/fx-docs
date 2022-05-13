---
description: >-
  The documentation corresponding contains details for the RPC - HTTP, WS and
  GRPC endpoints
---

# f(x)Core Network

#### f(x)Core supports different clients in order to support Cosmos and Ethereum/Web3 transactions and queries

| <p><br></p>                                                             | Description                                                                  | Default Port |
| ----------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ------------ |
| **Cosmos gRPC**                                                         | Query or send f(x)Core transactions using gRPC                               | `9090`       |
| **Cosmos restAPI**                                                      | Query or send f(x)Core transactions using restAPI                            | `1317`       |
| **Web3** [**JSON-RPC**](web3/)****                                      | Query Web3-formatted transactions and blocks or send Web3 txs using JSON-RPC | `8545`       |
| <p><strong></strong></p><p><strong>Web3 Websocket</strong></p>          | Subscribe to Web3 logs and events emitted in smart contracts.                | `8546`       |
| **Tendermint** [**RPC**](json-rpc-api.md)****                           | Subscribe to f(x)Core logs and events emitted in smart contracts.            | `26657`      |
| **Tendermint Websocket**                                                | Query transactions, blocks, consensus state, broadcast transactions, etc.    | `26657`      |
| **Command Line Interface (**[**CLI**](../f-x-core/installation.md)**)** | Query or send f(x)Core transactions using your Terminal or Console.          | N/A          |

#### Various clients, tools and end points available

{% tabs %}
{% tab title="f(x)Core Mainnet" %}
|                    |                                                                |
| ------------------ | -------------------------------------------------------------- |
| ChainId            | `fxcore`                                                       |
| Native Coin        | `FX`                                                           |
| Block Explorer     | [https://explorer.functionx.io](https://explorer.functionx.io) |
| JSON RPC           | https://fx-json.functionx.io:26657                             |
| JSON RPC Websocket | wss://fx-json.functionx.io:26657/websocket                     |
| gRPC               | https://fx-grpc.functionx.io:9090                              |
{% endtab %}

{% tab title="f(x)Core Testnet" %}
|                         |                                                                                                                                                                                                     |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ChainId                 | `dhobyghaut`                                                                                                                                                                                        |
| Native Coin             | `FX`                                                                                                                                                                                                |
| Block Explorer          | <p><a href="https://testnet-explorer.functionx.io">https://testnet-explorer.functionx.io</a><br><a href="https://dhobyghaut-explorer.functionx.io">https://dhobyghaut-explorer.functionx.io</a></p> |
| JSON RPC                | https://testnet-fx-json.functionx.io:26657                                                                                                                                                          |
| JSON RPC Websocket      | wss://testnet-fx-json.functionx.io:26657/websocket                                                                                                                                                  |
| gRPC                    | https://testnet-fx-grpc.functionx.io:9090                                                                                                                                                           |
| EVM ChainID             | 90001                                                                                                                                                                                               |
| EVM Block Explorer      | [https://testnet-fxscan.functionx.io](https://testnet-fxscan.functionx.io)                                                                                                                          |
| Web3 JSON RPC           | https://testnet-fx-json-web3.functionx.io:8545                                                                                                                                                      |
| Web3 JSON RPC Websocket | wss://testnet-fx-json-web3.functionx.io:8546                                                                                                                                                        |
| Testnet Faucet          | [https://dhobyghaut-faucet.functionx.io/](https://dhobyghaut-faucet.functionx.io)                                                                                                                   |
{% endtab %}
{% endtabs %}
