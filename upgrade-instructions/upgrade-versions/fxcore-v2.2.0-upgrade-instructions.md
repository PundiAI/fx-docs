---
description: >-
  This document describes the steps for validator and full node operators for
  the successful execution of the f(x)Core v2.2.0 Upgrade
---

# f(x)Core v2.2.0 Upgrade Instructions

### The upgrade contains the following main new feature/improvement:

**Bug Fixes**

* Fix transaction msg `MsgConvertCoin` `MsgConvertERC20` too much gas
* Fix crosschain to ethereum
* Fix tendermint subcommand

**New**

* Add query oracle reward in the cross-chain module
* Check fxcored version when synchronizing blocks from scratch
* Add denom many to one support
* Update RegisterCoinProposal support denom many to one
* Add UpdateDenomAliasProposal and MsgConvertDenom
* Enable [f(x)Core Gravity Cross-Chain Bridge](../../deploying-on-fxcore-evm/cross-chain-bridges/fxcore-gravity-bridge.md) to allow a many-to-one ERC20 token mapping feature.
  * This would allow tokens from different chains (registered on the bridge) to be bridged into f(x)Core forming a unified token which is chain agnostic.
  * Example: ETH-USDT, Polygon-USDT, Tron-USDT cross-chain through the Gravity Cross-Chain Bridge, then converted by the fx-ERC20 module, and unified into a FX-USDT ERC20 Token upon entering fx-evm.
*   Any addition of new tokens to this Gravity Cross-Chain Bridge feature logic would have to go through governance.

    * The proposal would need to be launched under the proposal type "**RegisterCoinProposal**"



![](<../../.gitbook/assets/Unified V2.drawio.png>)

> Note: You may refer to the latest news and releases information in the [official FunctionX Github](https://github.com/FunctionX/fx-core/releases)

{% hint style="info" %}
**Note:** Refer to [Binaries upgrade steps](../upgrade-guide/evm-upgrade-tutorial.md) or [Docker upgrade steps](../upgrade-guide/evm-upgrade-tutorial-docker.md)
{% endhint %}

### Testnet upgrade plan:

* Code release time: The Testnet code is expected to be released by 6pm (GMT+8) on Friday, 22 July 2022.
* Upgrading validator nodes:
  * Validators can update their node once the code is released (please read about the difference between Software Upgrade and Hard Fork [here](../upgrade-guide/evm-upgrade-tutorial.md#f-x-core-network-upgrades)). The upgrade height will be written in the code. The upgrade height time will tentatively be set at 12pm (GMT+8) on Monday, 25 July 2022.
  * The validator must update the Testnet node before the upgrade height, otherwise it will result in issues with consensus because of the inconsistencies of versions with other validators.
* Initiate a many-to-one cross-chain proposal to add USDT
  * The node will not apply the update content until it reaches the upgrade height, thereafter, there will be a need to send a RegisterCoinProposal proposal after the upgrade height to add USDT to the list of tokens in the many-to-one cross-chain feature.
  * The Testnet proposal voting period has been changed from 2 days to 2 hours.
* The proposal is expected to pass at 2pm (GMT+8) on Monday, 25 July 2022.
* All tests on the testnet will be completed by Tuesday, 26 July 2022.

### Mainnet Upgrade Plan

* Code release time: The Mainnet code is expected to be released in the morning of Wednesday, 27 July 2022.
* Upgrading validator nodes:
  * Validators can update their node once the code is released (please read about the difference between Software Upgrade and Hard Fork [here](../upgrade-guide/evm-upgrade-tutorial.md#f-x-core-network-upgrades)). The upgrade height will be written in the code. The upgrade height time will tentatively be set at 12pm (GMT+8) on Monday, 1 August 2022.
  * The validator must update the Mainnet node before the upgrade height, otherwise it will result in issues with consensus because of the inconsistencies of versions with other validators
* Initiate a many-to-one cross-chain proposal to add USDT
  * The node will not apply the update content until it reaches the upgrade height, thereafter, there will be a need to send a RegisterCoinProposal proposal after the upgrade height to add USDT to the list of tokens in the many-to-one cross-chain feature.
  * Proposal cycle is 14 days
* The proposal is expected to pass on Monday, 15 August 2022.
