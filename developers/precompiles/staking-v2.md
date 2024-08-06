# Staking V2

Address: `0x0000000000000000000000000000000000001003`

Interface: [IStaking](https://github.com/FunctionX/fx-core/blob/main/solidity/contracts/staking/IStaking.sol)

ABI: [IStaking](https://github.com/FunctionX/fx-core/blob/main/contract/IStaking.go#L32)

## Introduction

use staking v2 precompiled contract to call some functions of the staking module, such as: delegate, undelegate, redelegate,
etc.

## Method

### delegateV2

delegate token to validator, get result

```solidity
function delegateV2(
    string memory _val,
    uint256 _amount
) external returns (bool _result);
```

* `_val`: the validator address
* `_amount`: the amount of the token to be delegate

* `_result_`: the delegate result

{% hint style="info" %}
only can delegate origin token
{% endhint %}

{% hint style="info" %}
only delegate validator who has participated in block generation, delegate again, will get reward
{% endhint %}

delegate event

```solidity
event DelegateV2(
    address indexed delegator,
    string validator,
    uint256 amount
);
```

* `delegator`: the delegator address
* `validator`: the validator address to be delegated
* `amount`: the amount of the token to be delegated

### undelegateV2

undelegate token from validator, get result

```solidity
function undelegateV2(
    string memory _val,
    uint256 _amount
) external returns (bool _result);
```

* `_val`: the validator address to be undelegate
* `_amount`: the amount to undelegate

* `_result_`: the undelegate result

undelegate event

```solidity
event UndelegateV2(
    address indexed sender,
    string validator,
    uint256 amount,
    uint256 completionTime
);
```

* `sender`: the sender address
* `validator`: the validator address to be undelegate
* `amount`: the amount to undelegate
* `completionTime`: the completion time of undelegate

### redelegateV2

redelegate token from validator to other validator, get result

```solidity
function redelegateV2(
    string memory _valSrc,
    string memory _valDst,
    uint256 _amount
) external returns (bool _result);
```

* `_valSrc`: the validator address to be redelegate
* `_valDst`: the validator address to be redelegate to
* `_amount`: the amount to redelegate

* `_result_`: the undelegate result


redelegate event

```solidity
event RedelegateV2(
    address indexed sender,
    string valSrc,
    string valDst,
    uint256 amount,
    uint256 completionTime
);
```

* `sender`: the sender address
* `valSrc`: the validator address to be redelegated
* `valDst`: the validator address to be redelegated to
* `amount`: the amount to redelegate
* `completionTime`: the completion time of redelegate
