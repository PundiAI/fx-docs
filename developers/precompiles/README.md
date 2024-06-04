# Precompiled Contracts

## Introduction

Precompiled contracts are specialized smart contracts designed to handle operations that are too complex or unsupported by Solidity, such as intricate mathematical or cryptographic functions. These contracts streamline certain processes by providing optimized implementations of such functions, making them more efficient and less resource-intensive.&#x20;

In the Ethereum Virtual Machine (EVM), most precompiled contracts have to be called directly using assembly or through: `precompiledAddress.staticcall(...)` in Solidity; with the `ecRecover` precompile being the only exception. In this instance, the `ecRecover` precompile can be invoked directly using the `ecrecover` keyword thanks to the wrapper function implemented by Solidity which simplifies calling the precompile.

EVM precompiled contracts are also available on f(x)Core; and in addition to those, f(x)Core also features more advanced functionalities such as delegations and cross-chain support through additional precompiled contracts - of which will be explored in this article. Like most precompiles on the EVM, precompiles on f(x)Core can only be called through assembly or the `staticcall` call in Solidity.

## EVM Precompiled Contracts

Precompiled contracts on the EVM exist at fixed addresses and can be called deterministically. The addresses start from 1, and increment for each precompile. One example of a precompile on the EVM is the `ecRecover`\`precompile. The contract can be found at the address: 1 (0x01), and it is used for recovering an address from a hash and a digital signature for that hash. Solidity provides a built-in function [`ecrecover`](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#mathematical-and-cryptographic-functions) that interfaces with the `ecRecover` precompile, it accepts a message along with the `r`, `s` and `v` parameters and returns the address that was used to sign the message.

```solidity
function verify(
    address addressTest, 
    bytes32 msgHash,
    uint8 v,
    bytes32 r,
    bytes32 s
) public view returns (bool) {
    //Use ECRECOVER to verify address
    return (ecrecover(msgHash, v, r, s) == (addressTest));
}

```

The following shows an example of calling another precompile: `SHA2-256`, which does not have a wrapper function provided by Solidity for invocation; in this case, inline-assembly must be used.

```solidity
function hashWithSha256(uint256 numberToHash) public view returns (bytes32) {
    assembly {
        mstore(0, numberToHash) // store arg in memory for `staticcall`
        let ok := staticcall(gas(), 2, 0, 32, 0, 32)
        if iszero(ok) {
            revert(0,0)
	}
	return(0, 32)
    }
}
```

## f(x)Core Precompiled Contracts

To enable contract-based invocation of more advanced functionalities on the f(x)Core chain, such as delegation, cross-chain etc., support for precompiled contracts was added to the EVM module on f(x)Core.

f(x)Core is built based on the cosmos-sdk, and contract data in the EVM module is stored separately. Thus, to implement precompiled contracts, state data must be synchronized between the cosmos-sdk and EVM module.&#x20;

The EVM module is a [fork](https://github.com/functionx/ethermint) of the ethermint project from Evmos, and modifications, among others, include changes to ensure compatibility with f(x)Core. Data in the EVM module is completely isolated from other cosmos-sdk modules, and calls to contracts can be directly made within other modules through the EVM module. However, initiating calls to functions from other f(x)Core modules from within a contract are not allowed; hooks must be set in the EVM module and contract to passively invoke advanced functionalities on f(x)Core.

After the implementation of the cross-chain precompiled contract, corresponding functions based on the precompiled contract only need to be added to ensure state synchronization. No matter how the call to the precompiled contract is made, atomicity can be guaranteed; and the precompiled contract can be directly called by other contracts or addresses, used in the same way as an ordinary contract.

## f(x)Core Specific Precompiles:

* [CrossChain Precompile](cross-chain.md)
* [Staking Precompile](staking.md)\
