---
description: >-
  There are 2 main ways of bridging tokens from other networks into
  f(x)Core-EVM, this document will cover both methods. How a user can use the
  bridge.
---

# Bridging Tokens from other networks

## For Developers

### Add tokens to the [f(x)Core Gravity Bridge](fxcore-gravity-bridge.md)

Bridging tokens from other networks (supported by the [f(x)Core Gravity Bridge](fxcore-gravity-bridge.md)) via [f(x)Core Gravity Bridge](fxcore-gravity-bridge.md). This would require the following steps:

1.  Deploy the token contract to fxcore-evm to support cross-chain

    Eg. EURS: https://etherscan.io/address/0xdB25f211AB05b1c97D595516F45794528a807ad8
2. Create a proposal on [f(x)Core network](https://explorer.functionx.io/fxcore/proposals)
   1. **Title**: \<Free to decide> eg. Register
   2. **Type**: Register Coin
   3. **Details**: \<Free to decide>
   4. **Metadata:**
      1. **description**: The cross chain token of the Function X
      2. **1st denom**: \<based on coin>
      3. **1st aliases**: \<based on coin>
      4. **2nd denom**: \<based on coin>
      5. **2nd exponent**: \<based on coin exponent>
      6. **2nd aliases**: \<based on coin, typically not needed>&#x20;
      7. **display**: \<based on coin>
      8. **name**: \<based on coin>
      9. **symbol**: \<based on coin>

Example:

![](../../.gitbook/assets/Register\_Coin\_Eg.png)

![](../../.gitbook/assets/Register\_Coin\_Eg2.png)

1. After the proposal passes, the Ethereum ERC20 address will be added to the Ethereum bridge contract
2. You will be able to perform cross-chain transactions

> More on[ f(x)Core governance](broken-reference)

### Building the bridge yourself

3rd party bridge created and deployed by oneself. This would typically require

1. A creation of the ERC20 token smart contract on FX-EVM
2. Creation and deployment of bridge

## For Users

f(x)Wallet will soon be able to support the Gravity Bridge
