---
description: >-
  This document describes the steps for validator and full node operators for
  the successful execution of the fxCore V2.1.0 Upgrade
---

# fxCore V2.1.0 Upgrade Instructions

### The upgrade contains the following main new features/improvement:

* Tendermint version from v0.34.9 to v0.34.20
* Cosmos-SDK version from v0.42.4 to v.45.5
* IBC version upgrade from cosmos-sdk/x/ibc to ibc-go v3.1.0
* New modules: feegrant, authz, feemarket, evm, erc20, migrate
* Migration modules: auth, bank, distribution, gov, slashing, ibc, crosschain (bsc, polygon, tron)
* Crosschain module upgrade: the original oracle deposit will be automatically entrusted to the validator with the largest power value after the upgrade, the oracle can replace the entrusted validator by itself in the future, oracle needs to manually receive the entrusted reward
* [CLI Breaking Changes](https://github.com/FunctionX/fx-core/blob/release/v2.1.x/CHANGELOG.md#cli-breaking-changes)
* [API Breaking Changes](https://github.com/FunctionX/fx-core/blob/release/v2.1.x/CHANGELOG.md#api-breaking-changes)
* [Features](https://github.com/FunctionX/fx-core/blob/release/v2.1.x/CHANGELOG.md#features)
* [Bug Fixes](https://github.com/FunctionX/fx-core/blob/release/v2.1.x/CHANGELOG.md#bug-fixes)
* More on the details of the upgrade and changes can be found in [CHANGELOG.MD](https://github.com/FunctionX/fx-core/blob/release/v2.1.x/CHANGELOG.md)

TOC:

* [On-chain governance proposal](fxcore-v2.1.0-upgrade-instructions.md#on-chain-governance-proposal-attains-consensus)
* [Upgrade will take place July 16, 2022](fxcore-v2.1.0-upgrade-instructions.md#upgrade-will-take-place-april-12-2022)
* [fxCore Chain-id will remain the same and the new EVM Chain-id will be 530](fxcore-v2.1.0-upgrade-instructions.md#chain-id-will-remain-the-same)

### On-chain governance proposal attains consensus <a href="#on-chain-governance-proposal-attains-consensus" id="on-chain-governance-proposal-attains-consensus"></a>

[Proposal #10](https://explorer.functionx.io/fxcore/proposals/10) is the reference on-chain governance proposal for this upgrade. Neither core developers nor core funding entities control the governance, and this governance proposal is  conducted in a _fully decentralized_ way.

### Upgrade will take place July 16, 2022 <a href="#upgrade-will-take-place-april-12-2022" id="upgrade-will-take-place-april-12-2022"></a>

The upgrade will take place at a block height of `5,713,000`. At the time of writing, and at current block times (around 5.5s/block), this block height corresponds approximately to `Saturday, 16-July-22` 12:00:00 `UTC`. This date/time is approximate as blocks are not generated at a constant interval.

### fxCore Chain-id will remain the same and new EVM Chain-id will be 530 <a href="#chain-id-will-remain-the-same" id="chain-id-will-remain-the-same"></a>

The chain-id of the network will remain the same, `fxcore`. This is because an in-place migration of state will take place, i.e., this upgrade does not export any state.

However, with the additional of the new EVM module, the corresponding EVM Chain-id will be 530.

### Preparing for the upgrade <a href="#preparing-for-the-upgrade" id="preparing-for-the-upgrade"></a>

#### System requirements <a href="#system-requirement" id="system-requirement"></a>

For a smooth upgrade, ensure that you have the [minimum system requirements](../../f-x-core/installation.md#hardware-requirements).

#### Backups <a href="#backups" id="backups"></a>

It is critically important for validator operators to back-up the `.fxcore/data/priv_validator_state.json` file after stopping the fxcore process. This file is updated every block as your validator participates in consensus rounds. It is a critical file needed to prevent double-signing, in case the upgrade fails and the previous chain needs to be restarted.

Also validator operators should back-up their `.fxcore/config/`priv\_validator\_key`.json` file so they will be able to recover their validators in case the upgrade fails. More on the [validator recovery process](../../validators/validator-recovery.md).

#### Testing <a href="#testing" id="testing"></a>

For those validator and full node operators that are interested in ensuring preparedness for the impending upgrade, you can join in our public-testnet by [installing f(x)Core](../../f-x-core/installation.md) (ensure that you click into the testnet installation tabs).

#### Manual Upgrade

You may refer to this [upgrade tutorial](broken-reference) to upgrade your nodes.

#### Upgrade failure handling method

> A detailed upgrade failure handling tutorial will be added later

You may refer to our [official Github release page](https://github.com/FunctionX/fx-core/releases) for more information.

### On-Chain Upgrade Process

When the network reaches the [upgrade height](fxcore-v2.1.0-upgrade-instructions.md#upgrade-will-take-place-april-12-2022), the state machine program of fxCore will be halted. The classic method for upgrading requires all validators and node operators to manually substitute the existing state machine binary with the new binary. Because it is an on-chain upgrade process, the blockchain will be resumed with all the accumulated history and a rolling block height.

### Potential Risk Factors

Although there have been extensive testing and simulation, there will always be a risk that fxCore might experience problems due to potential bugs or errors from the new features. In the case of critical issues, validators should stop the operation of their nodes immediately. fxCore Contributors will coordinate with validators in the `#validators-verified channel` to create and execute a contingency plan. Likely this will be an emergency release with fixes or the recommendation to consider the upgrade aborted and revert back to the previous release.

As a validator performing the upgrade procedure on your consensus nodes carries a heightened risk of double-signing and being slashed. The most important piece of this procedure is verifying your software version and genesis file hash before starting your validator and signing.

The riskiest thing a validator can do is discover that they made a mistake and repeat the upgrade procedure again during the network startup. If you discover a mistake in the process, the best thing to do is wait for the network to start before correcting it.

### Communications <a href="#communications" id="communications"></a>

Operators are encouraged to join the

* Validator telegram group
* Or communicate through the forum for
  * [f(x)Core Mainnet Validator General Discussion & Enquiries](https://forum.functionx.io/t/f-x-core-mainnet-validator-general-discussion-enquiries/2142)
  * [f(x)Core Mainnet Validator Updates](https://forum.functionx.io/t/f-x-core-mainnet-validator-updates/2141)
  * [f(x)Core Mainnet Validator Bugs/Fixes & Technical Support](https://forum.functionx.io/t/f-x-core-mainnet-validator-bugs-fixes-technical-support/2140)
  * [https://forum.functionx.io/t/fx-evm-mainnet/4281/3](https://forum.functionx.io/t/fx-evm-mainnet/4281/3)
* Receive updates on releases in our [official Github](https://github.com/FunctionX/fx-core/releases)

