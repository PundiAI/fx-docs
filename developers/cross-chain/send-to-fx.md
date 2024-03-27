# sendToFx

sendToFx cross chain means that the contract deployed on other chains communicates with f(x)Core through cross chain
bridge to realize cross chain with f(x)Core.

supported external chains:

* Ethereum
* Binance Smart Chain
* Polygon
* Tron
* Avalanche
* Optimism
* Arbitrum

## Bridge Contract

{% tabs %}
{% tab title="Mainnet" %}

| Chain               | Contract Address                                                                                                         |
|:--------------------|:-------------------------------------------------------------------------------------------------------------------------|
| Ethereum            | [0x6f1D09Fed11115d65E1071CD2109eDb300D80A27](https://etherscan.io/address/0x6f1D09Fed11115d65E1071CD2109eDb300D80A27)    |
| Binance Smart Chain | [0x84238c00c8313920826D798e3cF6793Ef4F610ad](https://bscscan.com/address/0x84238c00c8313920826D798e3cF6793Ef4F610ad)     |
| Polygon             | [0x4052eA614c5a96631C546d8B8d323a7C3D9ABb69](https://polygonscan.com/address/0x4052eA614c5a96631C546d8B8d323a7C3D9ABb69) |
| Tron                | [TKLDunqAynYcXRktcYHqncp3R6nJeEXLd5](https://tronscan.org/#/address/TKLDunqAynYcXRktcYHqncp3R6nJeEXLd5)                  |
| Avalanche           | [0xDc3706d164d2D88b85ed702079ab331e2476D475](https://snowtrace.io/address/0xDc3706d164d2D88b85ed702079ab331e2476D475)    |

{% endtab %}

{% tab title="Testnet" %}

| Chain               | Contract Address                                                                                                                      |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| Ethereum            | [0xd384a8e8822Ea845e83eb5AA2877239150615C18](https://sepolia.etherscan.io/address/0xd384a8e8822ea845e83eb5aa2877239150615c18)          |
| Binance Smart Chain | [0x4C0974BC7e3911a0682a1E17F0faD59A2eF4A637](https://testnet.bscscan.com/address/0x4C0974BC7e3911a0682a1E17F0faD59A2eF4A637)          |
| Polygon             | [0x57b1E4C85B0f141aDE38b5573907BA8eF9aC2298](https://mumbai.polygonscan.com/address/0x57b1E4C85B0f141aDE38b5573907BA8eF9aC2298)       |
| Tron                | [TAXdkUAde6ztmJyQqANL4jwDYab8D9shQs](https://nile.tronscan.org/#/address/TAXdkUAde6ztmJyQqANL4jwDYab8D9shQs)                          |
| Avalanche           | [0xe79893e4218a9d855Ef6beeA8366119b92007d64](https://testnet.snowtrace.io/address/0xe79893e4218a9d855Ef6beeA8366119b92007d64)         |
| Optimism            | [0xCA1c58AF317C88Cdb41d4AB17B50592771356E69](https://sepolia-optimism.etherscan.io/address/0xca1c58af317c88cdb41d4ab17b50592771356e69) |
| Arbitrum            | [0xCA1c58AF317C88Cdb41d4AB17B50592771356E69](https://sepolia.arbiscan.io/address/0xca1c58af317c88cdb41d4ab17b50592771356e69)           |

{% endtab %}
{% endtabs %}

## Method

### sendToFx

send token from external chain to f(x)Core

{% hint style="info" %}
must be approved before calling this method
{% endhint %}

```solidity
function sendToFx(address _tokenContract, bytes32 _destination, bytes32 _targetIBC, uint256 _amount) external;
```

* `_tokenContract`: the contract address of the token on the external chain
* `_destination`: the destination address on the f(x)Core chain or other cosmos chain
* `_targetIBC`: the target of the token on the f(x)Core chain(more details see [Target](./target.md))
    * _targetIBC represents cross chain destination, such as `f(x)Core`, `Pundix`, `Marginx`
    * destination f(x)Core: _targetIBC is empty
    * destination erc20 token: _targetIBC is `erc20`
    * destination other cosmos chain(example Pundix): _targetIBC is `ibc/{id}/{prefix}`.
        * `id` is the channel id of the target chain on f(x)Core(examples: Pundix: 0)
        * `prefix` is the address prefix of the target chain(examples: Pundix: px)
* `_amount`: the amount of the token to be cross chain

{% hint style="info" %}
_destination convert address to bytes, if less than 32, fill in 0 in front, until 32 bytes
{% endhint %}

{% hint style="info" %}
_targetIBC convert string to bytes, if less than 32, fill in 0 in behind, until 32 bytes
{% endhint %}

### getBridgeTokenList

get cross chain token list

```solidity
function getBridgeTokenList() external view returns (BridgeToken[] memory);
```

BridgeToken struct

```solidity
struct BridgeToken {
    address addr;
    string name;
    string symbol;
    uint8 decimals;
}
```

### checkAssetStatus

check if the token is supported cross chain

```solidity
function checkAssetStatus(address _tokenAddr) external view returns (bool status);
```

* `_tokenAddr`: the contract address of the token on the external chain

* `status`: true means good condition


