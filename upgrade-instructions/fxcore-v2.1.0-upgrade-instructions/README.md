---
description: >-
  This document describes the steps for validator and full node operators for
  the successful execution of the fxCore V2.1.1 Upgrade
---

# fxCore V2.1.1 Upgrade Instructions

### The upgrade contains the following main new features/improvement:

{% hint style="info" %}
For changes and improvements of the previous version, you may refer to [**fxCore V2.1.0 (since been deprecated)**](fxcore-v2.1.0-since-been-deprecated.md)****
{% endhint %}

**Bug Fixes**

* Add support for the `x-crisis-skip-assert-invariants` CLI flag to the `start` command
* CLI parse legacy proposal `InitCrossChainParamsProposal` failed
* Deleted Polygon(USDT) and Tron(USDT) contracts and metadata initialization during migration and upgrade

**Improvements**

* Refactor gravity handle FxOriginatedTokenClaim

TOC:

* [On-chain governance proposal](./#on-chain-governance-proposal-attains-consensus)
* [Upgrade will take place July 16, 2022](./#upgrade-will-take-place-april-12-2022)
* [fxCore Chain-id will remain the same and the new EVM Chain-id will be 530](./#chain-id-will-remain-the-same)
* [Preparing for the upgrade](./#preparing-for-the-upgrade)
  * [System requirements](./#system-requirement)
  * [Backups](./#backups)
  * [Testing](./#testing)
  * [Manual Upgrade](./#manual-upgrade)
  * [Upgrade failure handling method](./#upgrade-failure-handling-method)
* [On-Chain Upgrade Process](./#on-chain-upgrade-process)
* [Upgrade duration](./#upgrade-duration)
* [Rollback plan](./#rollback-plan)
* [Potential Risk Factors](./#potential-risk-factors)
* [Communications](./#communications)

### On-chain governance proposal attains consensus <a href="#on-chain-governance-proposal-attains-consensus" id="on-chain-governance-proposal-attains-consensus"></a>

[Proposal #10](https://explorer.functionx.io/fxcore/proposals/10) is the reference on-chain governance proposal for this upgrade. Neither core developers nor core funding entities control the governance, and this governance proposal is  conducted in a _fully decentralized_ way.

### Upgrade will take place July 16, 2022 <a href="#upgrade-will-take-place-april-12-2022" id="upgrade-will-take-place-april-12-2022"></a>

The upgrade will take place at a block height of `5,713,000`. At the time of writing, and at current block times (around 5.5s/block), this block height corresponds approximately to `Saturday, 16-July-22` 12:00:00 `UTC`. This date/time is approximate as blocks are not generated at a constant interval.

{% hint style="warning" %}
Validators must upgrade within a 20,000 block timeframe if not they risk being slashed. More on [slashing conditions](../../validators/validator-faq.md#what-are-the-slashing-conditions).
{% endhint %}

### fxCore Chain-id will remain the same and new EVM Chain-id will be 530 <a href="#chain-id-will-remain-the-same" id="chain-id-will-remain-the-same"></a>

The chain-id of the network will remain the same, `fxcore`. This is because an in-place migration of state will take place, i.e., this upgrade does not export any state.

However, with the additional of the new EVM module, the corresponding EVM Chain-id will be 530.

### Preparing for the upgrade <a href="#preparing-for-the-upgrade" id="preparing-for-the-upgrade"></a>

#### System requirements <a href="#system-requirement" id="system-requirement"></a>

For a smooth upgrade, ensure that you have the [minimum system requirements](../../f-x-core/installation.md#hardware-requirements).

> Minimum system requirements have an upgrade by 2 Cores and 4GB

#### Backups <a href="#backups" id="backups"></a>

It is critically important for validator operators to back-up the `.fxcore/data/priv_validator_state.json` file after stopping the fxcore process. This file is updated every block as your validator participates in consensus rounds. It is a critical file needed to prevent double-signing, in case the upgrade fails and the previous chain needs to be restarted.

Also validator operators should back-up their `.fxcore/config/`priv\_validator\_key`.json` file so they will be able to recover their validators in case the upgrade fails. More on the [validator recovery process](../../validators/validator-recovery.md).

#### Testing <a href="#testing" id="testing"></a>

For those validator and full node operators that are interested in ensuring preparedness for the impending upgrade, you can join in our public-testnet by [installing f(x)Core](../../f-x-core/installation.md) (ensure that you click into the testnet installation tabs).

#### Manual Upgrade

You may refer to this [upgrade tutorial](../../f-x-core-tutorials/evm-upgrade-tutorial.md) to upgrade your nodes.

#### Upgrade failure handling method

> A detailed upgrade failure handling tutorial will be added later

You may refer to our [official Github release page](https://github.com/FunctionX/fx-core/releases) for more information.

### On-Chain Upgrade Process

When the network reaches the [upgrade height](./#upgrade-will-take-place-april-12-2022), the state machine program of fxCore will be halted. The classic method for upgrading requires all validators and node operators to manually substitute the existing state machine binary with the new binary. Because it is an on-chain upgrade process, the blockchain will be resumed with all the accumulated history and a rolling block height.

### Upgrade duration

The upgrade may take a few minutes to several hours to complete because fxCore participants operate globally with differing operating hours and it may take some time for operators to upgrade their binaries and connect to the network.

### Rollback plan

During the network upgrade, the core fxCore team will be keeping an ever vigilant eye and communicating with operators on the status of their upgrades. During this time, the core team will be attentive to operator needs to determine if the upgrade is experiencing unintended challenges. The core team would provide technical support during the time of the upgrade. In the event of unexpected challenges, the core team, after conferring with operators and attaining social consensus, may choose to declare that the upgrade will be skipped.

Steps to skip this upgrade proposal are simply to resume the fxCore network with the (downgraded) v1.0.x binary using the following command:

```shell
fxcored start --unsafe-skip-upgrade 5713000
```

> There is no particular need to restore a state snapshot prior to the upgrade height, unless specifically directed by core fxCore team.

{% hint style="info" %}
A social consensus decision to skip the upgrade will be based solely on technical merits, thereby respecting and maintaining the decentralized governance process of the upgrade proposal's successful YES vote.
{% endhint %}

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
