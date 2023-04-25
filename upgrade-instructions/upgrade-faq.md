# Upgrade FAQ

## What is the difference between hard fork and software upgrade?

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

## f(x)Core Upgrade

* Where is the default installation by make install for fxcored?
  * The default installation will be at $GOPATH/bin/fxcored
* How to determine whether the node upgrade is successful?
  * It can be seen that the node outputs the following log information. At the same time, the node does not have any panic information, and there is no other error information except the error output by the p2p module, indicating that the node upgrade and startup are completed.

```
10:29AM INF Starting Node service impl=Node server=node
10:29AM INF Starting P2P Switch service impl="P2P Switch" module=p2p server=node
10:29AM INF Starting PEX service impl=PEX module=pex server=node
10:29AM INF Starting AddrBook service book=fxcore/config/addrbook.json impl=AddrBook module=p2p server=node
10:29AM INF Starting RPC HTTP server on [::]:26657 module=rpc-server server=node 10:29AM INF Starting Mempool service impl=Mempool module=mempool server=node
10:29AM INF Starting BlockchainReactor service impl=BlockchainReactor module=blockchain server=node
10:29AM INF Starting BlockPool service impl=BlockPool module=blockchain server=node
```

* What should I do if the "`panic: Failed to start consensus state: found signature from the same key`" appears in log?
  * We should first check whether the `priv_validator_key.json` configured by the node is used by its node, if not, run the command `fxcored config config.toml consensus.double_sign_check_height 0`, to modify the double-sign check, and then restart the node
* How to determine whether the f(x)core mainnet genesis is correct
  * `md5sum $HOME/.fxcore/config/genesis.json`
  * `ded64cf0d1e556b7fd4577cfd44cc328`
* Is there any CLI command to check if a validator node is validating a block without checking the resource manager?
  * Query the consensus signature address used by the local node
    * `fxcored status -o json | jq .ValidatorInfo`
* Query the signature status of the local node
  * `fxcored query slashing signing-info $(fxcored tendermint show-validator)`
* Query the validator‘s signature status
  * `fxcored query slashing signing-infos`

## Rollback plan

During the network upgrade, the core fxCore team will be keeping an ever vigilant eye and communicating with operators on the status of their upgrades. During this time, the core team will be attentive to operator needs to determine if the upgrade is experiencing unintended challenges. The core team would provide technical support during the time of the upgrade. In the event of unexpected challenges, the core team, after conferring with operators and attaining social consensus, may choose to declare that the upgrade will be skipped.

Steps to skip this upgrade proposal are simply to resume the fxCore network with the (downgraded) binary using the following command:

```shell
fxcored start --unsafe-skip-upgrade <block_height>
```

> There is no particular need to restore a state snapshot prior to the upgrade height, unless specifically directed by core fxCore team.

{% hint style="info" %}
A social consensus decision to skip the upgrade will be based solely on technical merits, thereby respecting and maintaining the decentralized governance process of the upgrade proposal's successful YES vote.
{% endhint %}

## fxCore Gravity cross-chain

* How are cross-chain tokens converted on different chains?
  * FX Token is a native token on the fxCore chain (FX is generated from the fxCore chain), so FX ERC20 is burn in the Ethereum contract and unlock in fxCore;
  * PURSE Token is a native token on the Pundix chain (PURSE is generated in the Pundix chain), so PURSE is burn in the BSC contract and unlock on the pundix chain
  * Other non-native tokens are locked in the contract on the counterparty chain and mint on the fxCore chain
* Can users choose to cross-chain ERC20 FX Token to fx-native or WFX?
  * Yes, users can choose to cross-chain the FX token of Ethereum to the FX-native token on the fxCore chain or the WFX in the fxCore EVM contract. The WFX here is the same as the WETH of Ethereum. It is to convert the native token into conforming to the ERC20 token specification, in order to be more convenient to apply in defi scenarios such as uniswap

## Evm

* Can users currently link fxevm using metamask?
  * Yes

## Ledger

* How can I generate Cosmos eth\_secp256k1 keys with Ledger?

Ether eth\_secp256k1 keys are not supported on f(x)Core with Ledger. Only Cosmos keys (secp256k1) can be generated with Ledger.

* I can’t generate keys using the CLI with fxcored with the --ledger flag

CLI bindings with fxcored binary are not currently supported. In the meantime, you can use the Cosmos Ledger App with EIP712 using evmos.me (opens new window). See the EIP712 Signing section for reference.

* I can’t generate a key for the f(x)Core native multisig using the fxcored CLI and and Ledger

You can generate a multisig wallet using the fxcored CLI, although the --ledger option is not available at the moment.

* I can’t use Metamask or Keplr with the Cosmos Ledger app

Since f(x)Core only support Cosmos keys and uses the same HD path as Cosmos, the Cosmos Ledger app doesn’t work to sign Ether transactions.

## Countdown Timer

Where can i check the upgrade countdown timer?

{% embed url="https://functionx.github.io/fx-core/tools/countdown.html" %}
![](<../.gitbook/assets/image (21).png>)
{% endembed %}

We will be using this link for both Mainnet and Testnet.
