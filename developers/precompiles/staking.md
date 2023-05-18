# Staking

Address: `0x0000000000000000000000000000000000001003`

Interface: [IStaking](https://github.com/FunctionX/fx-core/blob/main/solidity/contracts/staking/IStaking.sol)

## Introduction

use staking precompiled contract to call some functions of the staking module, such as: delegate, undelegate, withdraw,
etc.

## Method

### delegate

delegate token to validator, get shares and reward

{% hint style="info" %}
delegate to different validators, the shares obtained are essentially different and cannot be added
{% endhint %}

```solidity
function delegate(
    string memory _val
) external payable returns (uint256 _shares, uint256 _reward);
```

* `msg.value`: payable method, the amount of the token to be delegated
* `_val`: the validator address

* `_shares`: the shares of the delegate token
* `_reward`: the reward of the delegate

{% hint style="info" %}
only can delegate origin token, must be input msg.value
{% endhint %}

{% hint style="info" %}
only delegate validator who has participated in block generation, delegate again, will get reward
{% endhint %}

delegate event

```solidity
event Delegate(
    address indexed delegator,
    string validator,
    uint256 amount,
    uint256 shares
);
```

* `delegator`: the delegator address
* `validator`: the validator address to be delegated
* `amount`: the amount of the token to be delegated
* `shares`: the shares of the delegate token

### undelegate

undelegate token from validator, get token, reward, and completion time

```solidity
function undelegate(
    string memory _val,
    uint256 _shares
)
external
returns (uint256 _amount, uint256 _reward, uint256 _completionTime);
```

* `_val`: the validator address to be undelegated
* `_shares`: the shares to undelegate

* `_amount`: the amount of the shares undelegated
* `_reward`: the reward of undelegated
* `_completionTime`: the completion time of undelegated

undelegate event

```solidity
event Undelegate(
    address indexed sender,
    string validator,
    uint256 shares,
    uint256 amount,
    uint256 completionTime
);
```

* `sender`: the sender address
* `validator`: the validator address to be undelegated
* `shares`: the shares to undelegate
* `amount`: the amount of the shares undelegated
* `completionTime`: the completion time of undelegated

### withdraw

withdraw delegate reward

```solidity
function withdraw(string memory _val) external returns (uint256 _reward);
```

* `_val`: the validator address to be withdraw

* `_reward`: reward amount

withdraw event

```solidity
event Withdraw(address indexed sender, string validator, uint256 reward);
```

* `withdrawer`: the withdraw address
* `validator`: the validator address to be withdraw
* `reward`: reward amount

### approveShares

approve transferFrom shares amount

```solidity
function approveShares(
    string memory _val,
    address _spender,
    uint256 _shares
) external returns (bool _result);
```

* `_val`: the validator address to be approved
* `_spender`: the spender address
* `_shares`: the shares amount to be approved

* `_result`: the result of approve

approveShares event

```solidity
event ApproveShares(
    address indexed owner,
    address indexed spender,
    string validator,
    uint256 shares
);
```

* `owner`: the shares owner address
* `spender`: the spender address
* `validator`: the validator address to be approved
* `shares`: the shares amount to be approved


### transferShares

transfer shares to other address

```solidity
function transferShares(
    string memory _val,
    address _to,
    uint256 _shares
) external returns (uint256 _token, uint256 _reward);
```

* `_val`: the validator address to be transfer
* `_to`: the receiver address
* `_shares`: the shares amount to be transfer

* `_token`: the token amount of transfer shares
* `_reward`: the reward amount of to address when transfer shares

transferShares event

```solidity
event TransferShares(
    address indexed from,
    address indexed to,
    string validator,
    uint256 shares,
    uint256 token
);
```

* `from`: the shares owner address
* `to`: the receiver address
* `validator`: the validator address to be transfer
* `shares`: the shares amount to be transfer
* `token`: the token amount of transfer shares

### transferFromShares

transfer shares with approval

{% hint style="info" %}
transfer shares from other address, need to approve first
{% endhint %}

```solidity
function transferFromShares(
    string memory _val,
    address _from,
    address _to,
    uint256 _shares
) external returns (uint256 _token, uint256 _reward);
```

* `_val`: the validator address to be transfer
* `_from`: the shares owner address
* `_to`: the receiver address
* `_shares`: the shares amount to be transfer

* `_token`: the token amount of transfer shares
* `_reward`: the reward amount of to address when transfer shares

transferFromShares event(same as transferShares)

```solidity
event TransferShares(
    address indexed from,
    address indexed to,
    string validator,
    uint256 shares,
    uint256 token
);
```

* `from`: the shares owner address
* `to`: the receiver address
* `validator`: the validator address to be transfer
* `shares`: the shares amount to be transfer
* `token`: the token amount of transfer shares

### delegation

query delegation

```solidity
function delegation(
    string memory _val,
    address _del
) external view returns (uint256 _shares, uint256 _delegateAmount);
```

* `_val`: the validator address to be query
* `_del`: the delegator address to be query

* `_shares`: the shares of the delegator in the validator
* `_delegateAmount`: the amount of the delegator in the validator

### delegationRewards

query delegation rewards

```solidity
function delegationRewards(
    string memory _val,
    address _del
) external view returns (uint256 _reward);
```

* `_val`: the validator address to be query
* `_del`: the delegator address to be query

* `_reward`: the reward of the delegator in the validator

### allowanceShares

query allowance shares

```solidity
function allowanceShares(
    string memory _val,
    address _owner,
    address _spender
) external view returns (uint256 _shares);
```

* `_val`: the validator address to be query
* `_owner`: the owner address to be query
* `_spender`: the spender address to be query

* `_shares`: the allowance shares in the validator