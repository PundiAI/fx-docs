# Cross Chain

Address: `0x0000000000000000000000000000000000001004`

Interface: [ICrossChain](https://github.com/FunctionX/fx-core/blob/main/solidity/contracts/crosschain/ICrossChain.sol)

## Introduction

use crossChain precompiled contract to call some functions of the crossChain module, such as: crossChain,
cancelSendToExternal, etc.

## Method

### crossChain

send token from f(x)Evm to other chain, like ethereum, pundix, etc.

{% hint style="info" %}
if token is erc20, must be approved before calling this method
{% endhint %}

```solidity
function crossChain(
    address _token,
    string memory _receipt,
    uint256 _amount,
    uint256 _fee,
    bytes32 _target,
    string memory _memo
) external payable returns (result bool);
```

* `_token`: the token address on the f(x)Evm([Support Token](../cross-chain/fx-evm.md))
    * if token is origin token, _token address is `0x0000000000000000000000000000000000000000`
* `_receipt`: the receipt address on the target chain
* `_amount`: the amount of the token to be cross chain
* `_fee`: the fee of the token to be cross chain
    * if cross chain to cosmos chain, _fee is `0`
* `_target`: the target of the token on the target chain(more detail see [Target](../cross-chain/target.md)
    * _target represents cross chain destination, such as chain `ethereum`, `pundix`
    * destination ethereum: _target is `eth`
    * destination other cosmos chain(example Pundix): _target is `ibc/{id}/{prefix}`.
        * `id` is the channel id of the target chain on f(x)Core(examples: Pundix: 0)
        * `prefix` is the address prefix of the target chain(examples: Pundix: px)
* `_memo`: the memo of cross chain

{% hint style="info" %}
_target convert string to bytes, if less than 32, fill in 0 in behind, until 32 bytes
{% endhint %}


crossChain event

```solidity
event CrossChain(
    address indexed sender,
    address indexed token,
    string denom,
    string receipt,
    uint256 amount,
    uint256 fee,
    bytes32 target,
    string memo
);
```

* `sender`: the sender address
* `token`: the token address on the f(x)Evm
* `denom`: the cross chain token denom
* `receipt`: the receipt address on the target chain
* `amount`: the amount of the token to be cross chain
* `fee`: the fee of the token to be cross chain
* `target`: the target of cross chain
* `memo`: the memo of cross chain

### cancelSendToExternal

cancel cross chain to external chain, like ethereum, bsc, etc.

{% hint style="info" %}
only cancel send to external chain(ethereum,bsc, etc.) transaction, can't cancel send to cosmos chain(pundix, cosmoshub,
etc.) transaction.
{% endhint %}

```solidity
function cancelSendToExternal(
    string memory _chain,
    uint256 _txid
) external returns (result bool);
```

* `_chain`: the module name of the cross chain, like eth, bsc, etc.
* `_txid`: the txid of the cross chain transaction

cancelSendToExternal event

```solidity
event CancelSendToExternal(
    address indexed sender,
    string chain,
    uint256 txID
);
```

* `sender`: the sender address
* `chain`: the module name of the cross chain, like eth, bsc, etc.
* `txID`: the txid of the cross chain transaction

### increaseBridgeFee

increase fee for send to external chain transaction

{% hint style="info" %}
only increase fee for send to external chain transaction, can't increase fee for send to cosmos chain transaction
{% endhint %}

{% hint style="info" %}
not necessarily the initiator of the cross chain transaction, anyone can increase the fee for the cross chain
transaction
{% endhint %}

{% hint style="info" %}
if token is erc20, must be approved before calling this method
{% endhint %}

```solidity
function increaseBridgeFee(
    string memory _chain,
    uint256 _txid,
    address _token,
    uint256 _fee
) external payable returns (result bool);
```

* `_chain`: the chain name of the cross chain, like ethereum, bsc, etc.
* `_txid`: the txid of the cross chain transaction
* `_token`: the address of cross chain fee token([Support Token](../cross-chain/fx-evm.md))
    * if token is origin token, _token address is `0x0000000000000000000000000000000000000000`
* `_fee`: increase fee amount


increaseBridgeFee event

```solidity
event IncreaseBridgeFee(
    address indexed sender,
    address indexed token,
    string chain,
    uint256 txID,
    uint256 fee
);
```

* `sender`: the sender address
* `token`: the address of cross chain fee token
* `chain`: the module name of the cross chain, like eth, bsc, etc.
* `txID`: the txid of the cross chain transaction
* `fee`: increase fee amount