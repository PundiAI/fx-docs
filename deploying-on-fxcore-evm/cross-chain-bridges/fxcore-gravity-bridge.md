---
description: >-
  This document will discuss everything there is to know about the f(x)Core
  Gravity Bridge
---

# f(x)Core Gravity Bridge

### What Is f(x)Core Gravity Bridge?

The f(x)Core Gravity Bridge is a trustless, secure and decentralised bridge between Ethereum and the Cosmos ecosystem. Powered by Cosmos SDK with the aid of smart contracts, the bridge uses the validator set to sign transactions, instead of a multi-sig or a permissioned set of actors.

The bridge allows the seamless addition of ERC20 tokens to the bridge, and thereafter the free movement of ERC20 tokens between the Ethereum and Cosmos ecosystem.

### Features

Simple by design, the f(x)Core Gravity Bridge eliminates design complexity and offers greater security.

* **Highly decentralised:**&#x20;
  * Movement of funds are controlled by validators
  * The addition and removal of validators in the group are controlled by the validators
  * The addition of new types of tokens to the bridge can be done through [governance](./bridging-tokens-from-other-networks)
* **Unified token:** The unification of the same token from different chains to form a single unified token on f(x)Core that is chain-agnostic
* **Secure:** Validators cannot sign or submit bridge messages not agreed upon by consensus
* **Gas Efficient:** Rollup-style batch transactions reduce individual users’ gas costs

### The Building Blocks of the f(x)Core Gravity Bridge

The Gravity Bridge has three defined components:

1. A Solidity contract on Ethereum
2. A Cosmos SDK module on the Gravity Bridge blockchain
3. A Cosmos SDK module: fxcore-ERC20

### How does it work?

Like most cross-chain bridges, the Gravity Bridge involves locking up a native token/coin on one side of the bridge and then minting a representation of that token on the other of the bridge. On the way back, the representation of the token is burnt and then unlocked on the native chain.

When a representation of the token is minted on the f(x)Core chain, it will form a unified token which is chain agnostic. Example: ETH-USDT, Polygon-USDT, Tron-USDT cross-chain through the Gravity Cross-Chain Bridge, then converted by the fx-ERC20 module, and unified into a FX-USDT ERC20 Token upon entering fx-evm. Below is a diagram to show how the bridge works.

![](<../../.gitbook/assets/Unified V2.drawio.png>)

### Deposits

Depositing assets to Gravity Bridge from Ethereum is permissionless and censorship-resistant. Each validator attests to every deposit event as they occur on Ethereum. When an event is attested by more than ⅔ of the validator set, representative tokens are minted. Validators must submit all attestations in order.

1. The user calls the sendToFx method of the bridge contract of an EVM-chain through f(x)wallet to lock the USDT in the bridge contract
2. The validator(s) listens to the sendToFx event in the bridge contract, then signs the event, and then submits the signed event to f(x)Core through a transaction. After the f(x)Core link is signed by 2/3 of the validators, it will automatically be signed by the user. The corresponding USDT is mint on the account

### Withdrawals

Gravity Bridge batches withdrawal transactions, placing multiple SendToEth messages together in an individual batch. This works like a rollup on Ethereum: Executing many transactions in a single shared context is far more efficient than doing so individually.

1. Users call the sendToEth transaction of the fxCore chain through f(x)wallet and apply for withdrawal&#x20;
2. One of the arbitrage relays will monitor the transactions that apply for withdrawal on the f(x)Core chain. When the bridgeFee paid by all withdrawal transactions is enough to offset the transaction fee of sending submitBatch on the Ethereum chain, it will send requestBatch transactions to the f(x)Core chain to package all sendToEth transactions&#x20;
3. When other relayers listen to the requestBatch transaction on the f(x)Core chain, they will sign the packaged transaction, and then the confirmBatch transaction will occur on the f(x)Core chain&#x20;
4. The arbitrage relay will then collect the withdrawal package signature information of all relays from the f(x)Core chain, and call the submitBatch method of the Ethereum contract. The submitBatch method will check the signature information of the relay, and verify that the signatures collected are greater than 2/3. Once verified, the contract will unlock USDT and transfer it to the user account, completing the withdrawal. The remaining bridge fee will be transferred to the arbitrage relay

### How to Use Gravity Bridge

**For users:** f(x)Wallet will soon be able to support the Gravity Bridge

**For developers:** You may refer to this document to learn how to add your token to the [Gravity Bridge](../../deploying-on-fxcore-evm/cross-chain-bridges/bridging-tokens-from-other-networks.md)
