---
description: >-
  There are 2 main ways of bridging tokens from other networks into
  f(x)Core-EVM, this document will cover both methods. How a user can use the
  bridge.
---

# Bridging Tokens from other networks

## For Developers

### Add tokens to the [f(x)Core Gravity Bridge](f-x-core-gravity-bridge.md)

Bridging tokens from other networks (supported by the [f(x)Core Gravity Bridge](f-x-core-gravity-bridge.md)) via [f(x)Core Gravity Bridge](f-x-core-gravity-bridge.md). This would require the following steps:

1. ERC20 token contract address is needed
2. Launch a governance proposal on [f(x)Core network](https://explorer.functionx.io/fxcore/proposals) specifying the type: **RegisterCoinProposal**
3. After the proposal passes,
   1. The corresponding contract will be deployed on f(x)Core-evm and
   2. The Ethereum ERC20 address will be added to the Ethereum bridge contract
4. You will be able to perform cross-chain transactions

> More on[ f(x)Core governance](broken-reference)

### Building the bridge yourself

3rd party bridge created and deployed by oneself. This would typically require

1. A creation of the ERC20 token smart contract on FX-EVM
2. Creation and deployment of bridge

## For Users

f(x)Wallet will soon be able to support the Gravity Bridge
