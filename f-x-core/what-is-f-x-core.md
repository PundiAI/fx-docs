# What is f(x)Core

`f(x)Core` is the name of the Cosmos SDK application for FunctionX.

* `fxcored`: The f(x)Core Daemon and command-line interface (CLI) runs a full-node of the `fxcored` application.

`f(x)Core` is built on the Cosmos SDK using the following modules:

* `x/auth`: Accounts and signatures.
* `x/bank`: Token transfers.
* `x/staking`: Staking logic.
* `x/mint`: Inflation logic.
* `x/distribution`: Fee distribution logic.
* `x/slashing`: Slashing logic.
* `x/gov`: Governance logic.
* `ibc-go/modules`: Inter-blockchain communication. Hosted in the `cosmos/ibc-go` repository.
* `x/params`: Handles app-level parameters.

About the FunctionX Core: The FunctionX Core also known as a Hub is the first Core to be launched on the FunctionX Network. The role of a Core is to facilitate transfers between blockchains. If a blockchain connects to a Core via IBC, it automatically gains access to all the other blockchains that are connected to that Core. The FunctionX Core is a public Proof-of-Stake chain. It's native coin is the FX.

Next, learn how to [install f(x)Core](../tutorials/installation.md).
