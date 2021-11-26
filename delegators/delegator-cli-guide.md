# Delegator CLI Guide

This document contains all the necessary information for delegators to interact with the f(x)Core through the Command-Line Interface (CLI).

It also contains instructions on how to manage accounts, restore accounts from the fundraiser and use a ledger nano device.

> **Very Important**: Please ensure that you follow the steps hereinafter carefully, as negligence in this process could result in a loss of your FX. Therefore, do read through the following instructions in their entirety before proceeding and reach out to us in case you need  any support.

> Please also note that you are about to interact with the f(x)Core, a blockchain technology containing highly experimental software. While the blockchain has been developed with state of the art technology and audited with utmost care, we may still expect to have issues, updates and bugs. Furthermore, interaction with blockchain technology requires advanced technical skills and always involves risks that are outside our control. By using the software, you confirm that you understand the inherent risks associated with cryptographic software and that the FunctionX team will not be held liable for potential damages arising out of the use of the software. Any use of this open source software released under the Apache 2.0 license is done at your own risk and on an "AS IS" basis, without warranties or conditions of any kind.

Please exercise extreme caution!

## Table of Contents

* [Installing `fxcored`](delegator-cli-guide.md#installing-fxcored)
* [f(x)Core Accounts](delegator-cli-guide.md#f\(x\)Core-Accounts)
  * [Restoring an Account from the Fundraiser](delegator-cli-guide.md#restoring-an-account-from-the-fundraiser)
  * [Creating an Account](delegator-cli-guide.md#creating-an-account)
* [Accessing the f(x)Core Network](delegator-cli-guide.md#accessing-the-f\(x\)Core-Network)
  * [Running Your Own Full-Node](delegator-cli-guide.md#running-your-own-full-node)
  * [Connecting to a Remote Full-Node](delegator-cli-guide.md#connecting-to-a-remote-full-node)
* [Setting Up `fxcored`](delegator-cli-guide.md#setting-up-fxcored)
* [Querying the State](delegator-cli-guide.md#querying-the-state)
* [Sending Transactions](delegator-cli-guide.md#sending-transactions)
  * [A Note on Gas and Fees](delegator-cli-guide.md#a-note-on-gas-and-fees)
  * [Bonding FX and Withdrawing Rewards](delegator-cli-guide.md#bonding-FX-and-withdrawing-rewards)
  * [Participating in Governance](delegator-cli-guide.md#participating-in-governance)
  * [Signing Transactions from an Offline Computer](delegator-cli-guide.md#signing-transactions-from-an-offline-computer)

## Installing `fxcored`

`fxcored`: This is the command-line interface (CLI) to interact with a `fxcored` full-node.

> **Please check that you have downloaded the latest stable release of `fxcored`**

\[**Download the binaries**]

[**Install from source**](https://github.com/FunctionX/fx-core)

> `fxcored` can be interacted with via a terminal. To open the terminal, follow these steps:

* **Windows**: `Start` > `All Programs` > `Accessories` > `Command Prompt`
* **MacOS**: `Finder` > `Applications` > `Utilities` > `Terminal`
* **Linux**: `Ctrl` + `Alt` + `T`

## f(x)Core Accounts

At the core of every f(x)Core account, there is a seed, which takes the form of a 12 or 24-words mnemonic. From this mnemonic, it is possible to create multiple f(x)Core accounts, i.e. pairs of private key/public key. This is called an HD wallet (see [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) for more information on the HD wallet specification).

```
     Account 0                         Account 1                         Account 2

+------------------+              +------------------+               +------------------+
|                  |              |                  |               |                  |
|    Address 0     |              |    Address 1     |               |    Address 2     |
|        ^         |              |        ^         |               |        ^         |
|        |         |              |        |         |               |        |         |
|        |         |              |        |         |               |        |         |
|        |         |              |        |         |               |        |         |
|        +         |              |        +         |               |        +         |
|  Public key 0    |              |  Public key 1    |               |  Public key 2    |
|        ^         |              |        ^         |               |        ^         |
|        |         |              |        |         |               |        |         |
|        |         |              |        |         |               |        |         |
|        |         |              |        |         |               |        |         |
|        +         |              |        +         |               |        +         |
|  Private key 0   |              |  Private key 1   |               |  Private key 2   |
|        ^         |              |        ^         |               |        ^         |
+------------------+              +------------------+               +------------------+
         |                                 |                                  |
         |                                 |                                  |
         |                                 |                                  |
         +--------------------------------------------------------------------+
                                           |
                                           |
                                 +---------+---------+
                                 |                   |
                                 |  Mnemonic (Seed)  |
                                 |                   |
                                 +-------------------+
```

The funds stored in an account are controlled by the private key. This private key is generated from the mnemonic using a one-way function. If you lose the private key, you can retrieve it using the mnemonic. However, if you lose the mnemonic, you will lose access to all the derived private keys. Likewise, if someone gains access to your mnemonic, they gain access to all the associated accounts.

> **Do not lose or share your 24-word mnemonic with anyone. To prevent theft or loss of funds, it is best to ensure that you keep multiple copies of your mnemonic, and store it in a safe, secure place that only you have access to. If someone has your mnemonic, they will be able to gain access to your private keys and control the accounts associated with them.**

The address is a public string with a human-readable prefix (e.g. `fx1hs3tfedle32zzr5dh38gzzfn9ak2f4a9je4pf6`) that identifies your account. When someone wants to send you funds, they send it to your address. It is virtually computationally impossible to derive the private key from a public address.

#### On a Ledger Device

At the core of a ledger device, there is a mnemonic used to generate accounts on multiple blockchains (including the f(x)Core). Usually, you will create a new mnemonic when you initialize your ledger device. However, it is possible to tell the ledger device to use a mnemonic provided by the user instead. Let us go ahead and see how you can input the mnemonic you obtained during the fundraiser as the seed of your ledger device.

> _NOTE: To do this, **it is preferable to use a brand new ledger device.**. Indeed, there can be only one mnemonic per ledger device. If, however, you want to use a ledger that is already initialized with a seed, you can reset it by going in `Settings`>`Device`>`Reset All`. **Please note that this will wipe out the seed currently stored on the device. If you have not properly secured the associated mnemonic, you could lose your funds!!!**_

The following steps need to be performed on an un-initialized ledger device:

1. Connect your ledger device to the computer via USB
2. Press both buttons
3. Do **NOT** choose the "Config as a new device" option. Instead, choose "Restore Configuration"
4. Choose a PIN
5. Choose the 12 words option
6. Input each of the words you got during the fundraiser, in the correct order.

Your ledger is now correctly set up with your fundraiser mnemonic! Do not lose this mnemonic! If your ledger is compromised, you can always restore a new device again using the same mnemonic.

Next, click [here](delegator-cli-guide.md#using-a-ledger-device) to learn how to generate an account.

#### On a Computer

> **NOTE: It is more secure to perform this action on an offline computer**

To restore an account using a fundraiser mnemonic and store the associated encrypted private key on a computer, use the following command:

```bash
fxcored keys add <yourKeyName> --recover
```

* `<yourKeyName>` is the name of the account. It is a reference to the account number used to derive the key pair from the mnemonic. You will use this name to identify your account when you want to send a transaction.
* You can add the optional `--account` flag to specify the path (`0`, `1`, `2`, ...) you want to use to generate your account. By default, account `0` is generated.

The private key of account `0` will be saved in your operating system's credentials storage. Each time you want to send a transaction, you will need to unlock your system's credentials store. If you lose access to your credentials storage, you can always recover the private key with the mnemonic.

> **You may not be prompted for password each time you send a transaction since most operating systems unlock user's credentials store upon login by default. If you want to change your credentials store security policies please refer to your operating system manual.**

### Creating an Account

To create an account, you just need to have `fxcored` installed. Before creating it, you need to know where you intend to store and interact with your private keys. The best options are to store them in an offline dedicated computer or a ledger device. Storing them on your regular online computer involves more risk, since anyone who infiltrates your computer through the internet could exfiltrate your private keys and steal your funds.

#### Using a Ledger Device

> **Only use Ledger devices that you bought factory new or trust fully**

When you initialize your ledger, a 24-word mnemonic is generated and stored in the device. This mnemonic is compatible with f(x)Core and f(x)Core accounts can be derived from it. Therefore, all you have to do is make your ledger compatible with `fxcored`. To do so, you need to go through the following steps:

1. Download the Ledger Live app [here](https://www.ledger.com/pages/ledger-live).
2. Connect your ledger via USB and update to the latest firmware
3. Go to the ledger live app store, and download the "Cosmos" application (this can take a while). **Note: You may have to enable `Dev Mode` in the `Settings` of Ledger Live to be able to download the "Cosmos" application**.
4. Navigate to the Cosmos app on your ledger device

Then, to create an account, use the following command:

```bash
fxcored keys add <yourAccountName> --ledger 
```

> **This command will only work while the Ledger is plugged in and unlocked**

* `<yourKeyName>` is the name of the account. It is a reference to the account number used to derive the key pair from the mnemonic. You will use this name to identify your account when you want to send a transaction.
* You can add the optional `--account` flag to specify the path (`0`, `1`, `2`, ...) you want to use to generate your account. By default, account `0` is generated.

#### Using a Computer

> **NOTE: It is more secure to perform this action on an offline computer**

To generate an account, just use the following command:

```bash
fxcored keys add <yourKeyName>
```

The command will generate a 24-words mnemonic and save the private and public keys for account `0` at the same time. Each time you want to send a transaction, you will need to unlock your system's credentials store. If you lose access to your credentials storage, you can always recover the private key with the mnemonic.

> **You may not be prompted for password each time you send a transaction since most operating systems unlock user's credentials store upon login by default. If you want to change your credentials store security policies please refer to your operating system manual.**

> **Do not lose or share your 12 words with anyone. To prevent theft or loss of funds, it is best to ensure that you keep multiple copies of your mnemonic, and store it in a safe, secure place and that only you know how to access. If someone is able to gain access to your mnemonic, they will be able to gain access to your private keys and control the accounts associated with them.**

> After you have secured your mnemonic (triple check!), you can delete bash history to ensure no one can retrieve it:

```bash
history -c
rm ~/.bash_history
```

* `<yourKeyName>` is the name of the account. It is a reference to the account number used to derive the key pair from the mnemonic. You will use this name to identify your account when you want to send a transaction.
* You can add the optional `--account` flag to specify the path (`0`, `1`, `2`, ...) you want to use to generate your account. By default, account `0` is generated.

You can generate more accounts from the same mnemonic using the following command:

```bash
fxcored keys add <yourKeyName> --recover --account 1
```

This command will prompt you to input a passphrase as well as your mnemonic. Change the account number to generate a different account.

## Accessing the f(x)Core Network

In order to query the state and send transactions, you need a way to access the network. To do so, you can either run your own full-node, or connect to someone else's.

> **NOTE: Do not share your mnemonic (12 or 24 words) with anyone. The only person who should ever need to know it is you. This is especially important if you are ever approached via email or direct message by someone requesting that you share your mnemonic for any kind of blockchain services or support. No one from f(x)Core, the Tendermint team or the Interchain Foundation will ever send an email that asks for you to share any kind of account credentials or your mnemonic."**.

### Running Your Own Full-Node

This is the most secure option, but comes with relatively high resource requirements. In order to run your own full-node, you need good bandwidth and at least 1TB of disk space.

You will find the tutorial on how to install `fxcored` [here](https://github.com/falcons-x/fx-docs/blob/master/tutorials/installation.md), and the guide to run a full-node [here](broken-reference).

### Connecting to a Remote Full-Node

If you do not want or cannot run your own node, you can connect to someone else's full-node. You should pick an operator you trust, because a malicious operator could return incorrect query results or censor your transactions. However, they will never be able to steal your funds, as your private keys are stored locally on your computer or ledger device. Possible options of full-node operators include validators, wallet providers or exchanges.

In order to connect to the full-node, you will need an address of the following form: `https://127.0.0.1:26657` (_Note: This is a placeholder_). This address has to be communicated by the full-node operator you choose to trust. You will use this address in the [following section](delegator-cli-guide.md#setting-up-fxcored).

## Setting Up `fxcored`

> **Before setting up `fxcored`, make sure you have set up a way to **[**access the f(x)Core network**](delegator-cli-guide.md#accessing-the-f\(x\)Core-Network)

> **Please check that you are always using the latest stable release of `fxcored`**

`fxcored` is the tool that enables you to interact with the node that runs on the f(x)Core network, whether you run it yourself or not. Let us set it up properly.

In order to set up `fxcored`, use the following command:

```bash
fxcored config <flag> <value>
```

It allows you to set a default value for each given flag.

First, set up the address of the full-node you want to connect to:

```bash
fxcored config node <host>:<port

// example: fxcored config node https://127.0.0.1:26657
```

If you run your own full-node, just use `tcp://localhost:26657` as the address.

Then, let us set the default value of the `--trust-node` flag:

```bash
fxcored config trust-node false

// Set to true if you run a light-client node, false otherwise
```

Finally, let us set the `chain-id` of the blockchain we want to interact with:

```bash
fxcored config chain-id dhobyghaut
```

## Querying the State

> **Before you can bond FX and withdraw rewards, you need to **[**set up `fxcored`**](delegator-cli-guide.md#setting-up-fxcored)

`fxcored` lets you query all relevant information from the blockchain, like account balances, amount of bonded tokens, outstanding rewards, governance proposals and more. Next is a list of the most useful commands for delegator.

```bash
// query account balances and other account-related information
fxcored query account <yourAddress>

// query the list of validators
fxcored query staking validators

// query the information of a validator given their address (e.g. fxvaloper1hs3tfedle32zzr5dh38gzzfn9ak2f4a96gg7h6)
fxcored query staking validator <validatorAddress>

// query all delegations made from a delegator given their address (e.g. fx1hs3tfedle32zzr5dh38gzzfn9ak2f4a9je4pf6)
fxcored query staking delegations <delegatorAddress>

// query a specific delegation made from a delegator (e.g. fx1hs3tfedle32zzr5dh38gzzfn9ak2f4a9je4pf6) to a validator (e.g. fxvaloper1hs3tfedle32zzr5dh38gzzfn9ak2f4a96gg7h6) given their addresses
fxcored query staking delegation <delegatorAddress> <validatorAddress>

// query the rewards of a delegator given a delegator address (e.g. fx1hs3tfedle32zzr5dh38gzzfn9ak2f4a9je4pf6)
fxcored query distribution rewards <delegatorAddress> 

// query all proposals currently open for depositing
fxcored query gov proposals --status deposit_period

// query all proposals currently open for voting
fxcored query gov proposals --status voting_period

// query a proposal given its proposalID
fxcored query gov proposal <proposalID>
```

For more commands, just type:

```bash
fxcored query
```

For each command, you can use the `-h` or `--help` flag to get more information.

## Sending Transactions

### A Note on Gas and Fees

Transactions on the f(x)Core network need to include a transaction fee in order to be processed. This fee pays for the gas required to run the transaction. The formula is the following:

```
fees = ceil(gas * gasPrices)
```

The `gas` is dependent on the transaction. Different transaction require different amount of `gas`. The `gas` amount for a transaction is calculated as it is being processed, but there is a way to estimate it beforehand by using the `auto` value for the `gas` flag. Of course, this only gives an estimate. You can adjust this estimate with the flag `--gas-adjustment` (default `1.0`) if you want to be sure you provide enough `gas` for the transaction. For the remainder of this tutorial, we will use a `--gas-adjustment` of `1.5`.

The `gasPrice` is the price of each unit of `gas`. Each validator sets a `min-gas-price` value, and will only include transactions that have a `gasPrice` greater than their `min-gas-price`.

The transaction `fees` are the product of `gas` and `gasPrice`. As a user, you have to input 2 out of 3. The higher the `gasPrice`/`fees`, the higher the chance that your transaction will get included in a block.

> For mainnet, the recommended `gas-prices` is `4000000000000FX`.

### Sending Tokens

> **Before you can bond FX and withdraw rewards, you need to **[**set up `fxcored`**](delegator-cli-guide.md#setting-up-fxcored)** and **[**create an account**](delegator-cli-guide.md#creating-an-account)

> **Note: These commands need to be run on an online computer. It is more secure to perform them commands using a Ledger Nano S device. For the offline procedure, click **[**here**](delegator-cli-guide.md#signing-transactions-from-an-offline-computer)**.**

```bash
// Send a certain amount of tokens to an address
// Ex value for parameters (do not actually use these values in your tx!!): <to_address>=fx1hs3tfedle32zzr5dh38gzzfn9ak2f4a9je4pf6 <amount>=1000000FX
// Ex value for flags: <gasPrice>=4000000000000FX

fxcored tx send <to_address> <amount> --from <yourKeyName> --gas auto --gas-adjustment 1.5 --gas-prices <gasPrice>
```

### Bonding FX and Withdrawing Rewards

> **Before you can bond FX and withdraw rewards, you need to **[**set up `fxcored`**](delegator-cli-guide.md#setting-up-fxcored)** and **[**create an account**](delegator-cli-guide.md#creating-an-account)

> **Before bonding FX, please read the **[**delegator faq**](broken-reference)** to understand the risk and responsibilities involved with delegating**

> **Note: These commands need to be run on an online computer. It is more secure to perform them commands using a ledger device. For the offline procedure, click **[**here**](delegator-cli-guide.md#signing-transactions-from-an-offline-computer)**.**

```bash
// Bond a certain amount of FX to a given validator

fxcored tx staking delegate <validatorAddress> <amountToBond> --from <delegatorKeyName> --gas auto --gas-adjustment 1.5 --gas-prices <gasPrice>


// Redelegate a certain amount of FX from a validator to another
// Can only be used if already bonded to a validator
// Redelegation takes effect immediately, there is no waiting period to redelegate
// After a redelegation, no other redelegation can be made from the account for the next 3 weeks

fxcored tx staking redelegate <srcValidatorAddress> <destValidatorAddress> <amountToRedelegate> --from <delegatorKeyName> --gas auto --gas-adjustment 1.5 --gas-prices <gasPrice>

// Withdraw all rewards

fxcored tx distribution withdraw-all-rewards --from <delegatorKeyName> --gas auto --gas-adjustment 1.5 --gas-prices <gasPrice>


// Unbond a certain amount of FX from a given validator 
// You will have to wait 3 weeks before your FX are fully unbonded and transferrable 

fxcored tx staking unbond <validatorAddress> <amountToUnbond> --from <delegatorKeyName> --gas auto --gas-adjustment 1.5 --gas-prices <gasPrice>
```

> **If you use a connected Ledger, you will be asked to confirm the transaction on the device before it is signed and broadcast to the network. Note that the command will only work while the Ledger is plugged in and unlocked.**

To confirm that your transaction went through, you can use the following queries:

```bash
// your balance should change after you bond FX or withdraw rewards
fxcored query account

// you should have delegations after you bond FX
fxcored query staking delegations <delegatorAddress>

// this returns your tx if it has been included
// use the tx hash that was displayed when you created the tx
fxcored query tx <txHash>
```

Double check with a block explorer if you interact with the network through a trusted full-node.

## Participating in Governance

#### Primer on Governance

The f(x)Core has a built-in governance system that lets bonded FX holders vote on proposals. There are three types of proposal:

* `Text Proposals`: These are the most basic type of proposals. They can be used to get the opinion of the network on a given topic.
* `Parameter Proposals`: These are used to update the value of an existing parameter.
* `Software Upgrade Proposal`: These are used to propose an upgrade of the Hub's software.

Any FX holder can submit a proposal. In order for the proposal to be open for voting, it needs to come with a `deposit` that is greater than a parameter called `minDeposit`. The `deposit` need not be provided in its entirety by the submitter. If the initial proposer's `deposit` is not sufficient, the proposal enters the `deposit_period` status. Then, any FX holder can increase the deposit by sending a `depositTx`.

Once the `deposit` reaches `minDeposit`, the proposal enters the `voting_period`, which lasts 2 weeks. Any **bonded** FX holder can then cast a vote on this proposal. The options are `Yes`, `No`, `NoWithVeto` and `Abstain`. The weight of the vote is based on the amount of bonded FX of the sender. If they don't vote, delegator inherit the vote of their validator. However, delegators can override their validator's vote by sending a vote themselves.

At the end of the voting period, the proposal is accepted if there are more than 50% `Yes` votes (excluding `Abstain `votes) and less than 33.33% of `NoWithVeto` votes (excluding `Abstain` votes).

#### In Practice

> **Before you can bond FX and withdraw rewards, you need to **[**bond FX**](delegator-cli-guide.md#bonding-FX-and-withdrawing-rewards)

> **Note: These commands need to be run on an online computer. It is more secure to perform them commands using a ledger device. For the offline procedure, click **[**here**](delegator-cli-guide.md#signing-transactions-from-an-offline-computer)**.**

```bash
// Submit a Proposal
// <type>=text/parameter_change/software_upgrade
// ex value for flag: <gasPrice>=4000000000000FX

fxcored tx gov submit-proposal --title "Test Proposal" --description "My awesome proposal" --type <type> --deposit=10000000FX --gas auto --gas-adjustment 1.5 --gas-prices <gasPrice> --from <delegatorKeyName>

// Increase deposit of a proposal
// Retrieve proposalID from $fxcored query gov proposals --status deposit_period
// ex value for parameter: <deposit>=10000000FX

fxcored tx gov deposit <proposalID> <deposit> --gas auto --gas-adjustment 1.5 --gas-prices <gasPrice> --from <delegatorKeyName>

// Vote on a proposal
// Retrieve proposalID from $fxcored query gov proposals --status voting_period 
// <option>=yes/no/no_with_veto/abstain

fxcored tx gov vote <proposalID> <option> --gas auto --gas-adjustment 1.5 --gas-prices <gasPrice> --from <delegatorKeyName>
```

### Signing Transactions From an Offline Computer

If you do not have a ledger device and want to interact with your private key on an offline computer, you can use the following procedure. First, generate an unsigned transaction on an **online computer** with the following command (example with a bonding transaction):

```bash
fxcored tx staking delegate <validatorAddress> <amountToBond> --from <delegatorAddress> \
--gas auto --gas-adjustment 1.5 --gas-prices <gasPrice> --generate-only > unsignedTX.json
```

In order to sign, you will also need the `chain-id`, `account-number` and `sequence`. The `chain-id` is a unique identifier for the blockchain on which you are submitting the transaction. The `account-number` is an identifier generated when your account first receives funds. The `sequence` number is used to keep track of the number of transactions you have sent and prevent replay attacks.

Get the chain-id from the genesis file, and the two other fields using the account query:

```bash
fxcored query account <yourAddress> --chain-id dhobyghaut
```

Then, copy `unsignedTx.json` and transfer it (e.g. via USB) to the offline computer. If it is not done already, [create an account on the offline computer](delegator-cli-guide.md#using-a-computer). For additional security, you can double check the parameters of your transaction before signing it using the following command:

```bash
cat unsignedTx.json
```

Now, sign the transaction using the following command. You will need the `chain-id`, `sequence` and `account-number` obtained earlier:

```bash
fxcored tx sign unsignedTx.json --from <delegatorKeyName> --offline --chain-id dhobyghaut --sequence <sequence> --account-number <account-number> > signedTx.json
```

Copy `signedTx.json` and transfer it back to the online computer. Finally, use the following command to broadcast the transaction:

```bash
fxcored tx broadcast signedTx.json
```
