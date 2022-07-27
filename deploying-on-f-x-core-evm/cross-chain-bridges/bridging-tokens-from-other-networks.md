---
description: >-
  There are 2 main ways of bridging tokens from other networks into
  f(x)Core-EVM, this document will cover both methods
---

# Bridging Tokens from other networks

### Using the [Cosmos Gravity Bridge](https://etherscan.io/address/0x9f5f3e25fc389ce62b1f8296272a1dfd05de2654#code)

Bridging tokens from other networks (supported by the [Cosmos Gravity Bridge](https://etherscan.io/address/0x9f5f3e25fc389ce62b1f8296272a1dfd05de2654#code)) via C[osmos Gravity Bridge](https://etherscan.io/address/0x9f5f3e25fc389ce62b1f8296272a1dfd05de2654#code). This would require the following steps:

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

### Overview of the Cosmos gravity bridge

It mainly implements the cross-chain mechanism between the smart contract (compatible with evm chain) and a cosmos chain. The tokens are locked or burned in the smart contract, and minted or unlocked in the cosmos chain.

Before the evm upgrade, the cross-chain token is stored in the fx prefix address account. After the upgrade, the user can choose to cross-chain the token to the fx-evm contract, so that the balance will be in the 0x address.

> At the moment, tron-USDT, polygon-USDT are not supported, the next version of f(x)Core will support it.
