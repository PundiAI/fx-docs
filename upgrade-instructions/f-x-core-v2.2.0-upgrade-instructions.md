---
description: >-
  This document describes the steps for validator and full node operators for
  the successful execution of the f(x)Core v2.2.0 Upgrade
---

# f(x)Core v2.2.0 Upgrade Instructions

### The upgrade contains the following main new feature/improvement:

* Enable Gravity Cross-Chain Bridge to allow a many-to-one ERC20 token mapping feature.
  * This would allow tokens from different chains (registered on the bridge) to be bridged into f(x)Core forming a unified token which is chain agnostic.
  * Example: ETH-USDT, Polygon-USDT, Tron-USDT cross-chain through the Gravity Cross-Chain Bridge, then converted by the fx-ERC20 module, and unified into a FX-USDT ERC20 Token upon entering fx-evm.
* Any addition of new tokens to this Gravity Cross-Chain Bridge feature logic would have to go through governance.
  * The proposal would need to be launched under the proposal type "**RegisterCoinProposal**"

![](<../.gitbook/assets/Unified ERC20.drawio.png>)

> Note: You may refer to the latest news and releases information in the [official FunctionX Github](https://github.com/FunctionX/fx-core/releases)

{% hint style="info" %}
**Note:** Refer to [Binaries upgrade steps](../f-x-core-tutorials/evm-upgrade-tutorial.md) or [Docker upgrade steps](../f-x-core-tutorials/evm-upgrade-tutorial-docker.md)
{% endhint %}

### Testnet upgrade plan:

* Code release time: The Testnet code is expected to be released by 4pm (GMT+8) on Friday, 22 July 2022.
* Upgrading validator nodes:
  * Validators can update their node once the code is released (please read about the difference between Software Upgrade and Hard Fork [here](../f-x-core-tutorials/evm-upgrade-tutorial.md#f-x-core-network-upgrades)). The upgrade height will be written in the code. The upgrade height time will tentatively be set at 12pm (GMT+8) on Monday, 25 July 2022.
  * The validator must update the Testnet node before the upgrade height, otherwise it will result in issues with consensus because of the inconsistencies of versions with other validators.
* Initiate a many-to-one cross-chain proposal to add USDT
  * The node will not apply the update content until it reaches the upgrade height, thereafter, there will be a need to send a RegisterCoinProposal proposal after the upgrade height to add USDT to the list of tokens in the many-to-one cross-chain feature.
  * The Testnet proposal voting period has been changed from 2 days to 2 hours.
* The proposal is expected to pass at 2pm (GMT+8) on Monday, 25 July 2022.
* All tests on the testnet will be completed by Tuesday, 26 July 2022.

### Mainnet Upgrade Plan

* Code release time: The Mainnet code is expected to be released in the morning of Wednesday, 27 July 2022.
* Upgrading validator nodes:
  * Validators can update their node once the code is released (please read about the difference between Software Upgrade and Hard Fork [here](../f-x-core-tutorials/evm-upgrade-tutorial.md#f-x-core-network-upgrades)). The upgrade height will be written in the code. The upgrade height time will tentatively be set at 12pm (GMT+8) on Monday, 1 August 2022.
  * The validator must update the Mainnet node before the upgrade height, otherwise it will result in issues with consensus because of the inconsistencies of versions with other validators
* Initiate a many-to-one cross-chain proposal to add USDT
  * The node will not apply the update content until it reaches the upgrade height, thereafter, there will be a need to send a RegisterCoinProposal proposal after the upgrade height to add USDT to the list of tokens in the many-to-one cross-chain feature.
  * Proposal cycle is 14 days
* The proposal is expected to pass on Monday, 15 August 2022.

### About upgrades: hard fork upgrades, software upgrades

* For validators:
  * The biggest difference between the two updates is what happens before and after the upgrade height. The hard fork method is to update the node before the upgrade height, and the software upgrade is to update the node after the upgrade height.
* For users:
  * There is not much difference between the two updates. The updated content will take effect through the proposal. The hard fork only adds new functions to the node first, but it will not be enabled immediately. To enable the new functions and features, a proposal will need to be launched after the upgrade.

Validator node hard fork upgrade

* Advantages:
  * No downtime, will not affect user use
  * Enough time for validators to update nodes
* Disadvantages:
  * Only simpler functions can be updated

Validator node software upgrade

* Advantages:
  * More secure
  * Maximize node performance
* Disadvantages:
  * Downtime, affecting user usage
  * Validator fails to update node in a timely manner and leads to jail time
