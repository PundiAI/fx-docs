---
description: This is work in progress. Mechanisms and values are susceptible to change.
---

# Validator FAQ

## General Concepts

### What is a validator?

[f(x)Core](../f-x-core/what-is-f-x-core.md) is based on [Tendermint](https://tendermint.com/docs/introduction/what-is-tendermint.html), which relies on a set of validators to secure the network. The role of validators is to run a full-node and participate in consensus by broadcasting votes which contain cryptographic signatures signed by their private key. Validators commit new blocks in the blockchain and receive revenue in exchange for their work. They must also participate in governance by voting on proposals. Validators are weighted according to their total stake.

### What is 'staking'?

The f(x)Core is a delgated Proof-Of-Stake (DPoS) blockchain, meaning that the weight of validators is determined by the total amount of staking tokens (FX) bonded as collateral. These FX can be self-delegated directly by the validator or delegated to them by other FX holders.

Any user in the system can declare their intention to become a validator by sending a `create-validator` transaction, provided they meet the miniminum self-delegated amount of 100FX. From there, they become validator candidates.

The weight (i.e. voting power) of a validator determines whether or not they are an active validator. Initially, only the top 50 validators with the most voting power will be active validators.

### What is a full-node?

A full-node is a program that fully validates transactions and blocks of a blockchain. It is distinct from a light-node that only processes block headers and a small subset of transactions. Running a full-node requires more resources than a light-node but is necessary in order to be a validator. In practice, running a full-node only implies running a non-compromised and up-to-date version of the software with low network latency and no downtime.

Of course, we encourage users to run full-nodes even if they do not plan to become validators.

### What is a delegator?

Delegators are FX holders who want to participate in protocol governance, but don’t want to carry the burden of becoming a validator. In which case they can delegate FX to a validator and obtain a slice of their revenue (as well as risks). (for more detail on how revenue is distributed, see [**What is the incentive to stake?**](validator-faq.md#what-is-the-incentive-to-stake) and [**What are validators commission?**](validator-faq.md#what-are-validators-commission) sections below).

Because they share revenue with their validators, delegators also share risks. Should a validator misbehave, each of their delegators will be partially slashed in proportion to their delegated stake. This is why delegators should perform their due diligence on validators before delegating, as well as spreading their stake over multiple validators.

Delegators play a critical role in the system, as they are responsible for choosing validators. Being a delegator is not a passive role: Delegators should actively monitor the actions of their validators and participate in governance. For more, read the [delegator's faq](broken-link/).

## Becoming a Validator

### How to become a validator?

Any participant in the network can signal that they want to become a validator by sending a `create-validator` transaction, where they must fill out the following parameters:

* **Validator's `PubKey`:** The validator's public key is your public key for your `validator's address`. The private key associated with this Tendermint `PubKey` is used to sign _prevotes_ and _precommits_.
* **Validator's Address:** Application level address. This is the address used to identify your validator publicly. The private key associated with this address is used to delegate, unbond, claim rewards, and participate in governance.
* **Validator's name (moniker):** This is the name of your validator that will be displayed on the interfaces.
* **Validator's website (Optional):** This is where delegators can find more information about this particular validator.
* **Validator's description (Optional):** Along with the name (moniker), this will be an easier form of identification for delegators.
* **Initial commission rate**: The commission rate on block rewards and fees that will be charged by validators and charged to delegators (more information can be found below).
* **Maximum commission:** The maximum commission rate which this validator can charge. This parameter cannot be changed after `create-validator` is processed.
* **Commission max change rate:** The maximum daily increase of the validator commission. This parameter cannot be changed after `create-validator` is processed.
* **Minimum self-delegation:** Minimum amount of FX the validator needs to have bonded at all time. If the validator's self-delegated stake falls below this limit, their entire staking pool will unbond.

Once a validator is created, FX holders can delegate FX to them, effectively adding stake to their pool. The total stake of an address is the combination of FX bonded by delegators and FX self-bonded by the validator.

The active validator set is determined solely by the ranking of the total amount staked. The 50 validators with the most total staked are the ones who are designated as **active validators**. If a validator's total stake falls below the top 50 then that validator loses their validator privileges: they don't participate in consensus and generate rewards any more. Over time, the maximum number of validators may increase via on-chain governance proposal.

## Testnet

### How can I join the testnet?

The Testnet is a great environment to test your validator setup before launch.

We view testnet participation as a great way to signal to the community that you are ready and able to operate a validator. You can find all relevant information about the testnet [here](broken-reference/).

### What are the different types of keys?

In short, there are two types of keys ([for more information on keys](../resources/f-x-cored-commands-documentation.md#keys)):

* **Operator Key**: This is a unique key used to sign consensus votes.
  * It is associated with a public key `fxvalconspub` (To get the public key, run the command `fxcored tendermint show-validator`).
  * It is generated when the node is created with fxcored init.
* **Account key**: This key is created from `fxcored` and used to sign transactions.
  * Account keys are associated with a public key prefixed by `fxpub` and an address prefixed by `fx`.
  * Both are generated by `fxcored keys add`.

> Note: A validator's operator key is directly tied to an application key, but uses the address (prefixed with`fxvaloper)` and public key (prefixed with `fxvaloperpub)` for consensus and governance purposes.

### What are the different states a validator can be in?

After a validator is created with a `create-validator` transaction, they can be in three states:

* `Active validator set`: Validator in the active set and participates in consensus. Validator is earning rewards and can be slashed for misbehaviour.
* `Jailed`: Validator misbehaved and is in jail, i.e. has been kicked out off the validator set. If the reason for being jailed is due to being offline for too long, the validator can send an `unjail` transaction in order to re-enter the active validator set. If the jailing is due to double signing, the validator cannot unjail.
* `Inactive`: Validator is not in the active set, and therefore not signing any blocks. Validator cannot be slashed, and does not earn any reward. It is still possible to delegate FX to this validator. Un-delegating from an `unbonded` validator is immediate and does not require a 21 day lockout period.

### What is 'self-delegation'? How can I increase my 'self-delegation'?

Self-delegation is delegation of FX of a validator from his own account. This amount can be increases by sending a `delegate` transaction from your validator's account.

### Is there a minimum amount of FX that must be delegated to be an active (=bonded) validator?

The minimum is `100FX`.

### How will delegators choose their validators?

Delegators are free to delegate to any validators according to their own criteria. Bottom line is that they must **DO YOUR OWN RESEARCH** and carry out their own due diligence on their chosen validator(s). That being said, the criterion we deem to be important include:

* **Amount of self-delegated FX:** Amount of self-delegated FX a validator has. A validator with a higher amount of self-delegated FX has more skin in the game, making them more accountable for their actions.
* **Total amount of delegated FX:** Total amount of FX delegated to a validator. The higher the amount of FX delegated to a validator, the higher the voting power of the validator. A higher voting power shows that the community trusts this validator, but it also means that this validator is a bigger target for hackers. Higher weighted validators also decrease the decentralisation of the network.
* **Commission rate:** Commission applied to the revenue of validators before it is distributed to their delegators.
* **Track record:** Points to note on the track record of a validator inlcudes seniority, past votes on proposals, historical average uptime and how often the node was compromised.

Apart from these criterion, validators can add a weblink to their validator account as a form of resume. Validators will need to build rapport within the community to attract delegators. For example, it would be a good practice for validators to have their setup audited by third parties. Note that the FunctionX team will not approve or conduct any audit themselves.

## Responsibilities

### Do validators need to be publicly identified?

No, they do not. Each delegator will assess validators based on their own criterion. Validators will be able to register a website address when they nominate themselves so that they can advertise their operation as they see fit. Some delegators may prefer a website that clearly displays the team operating the validator and their resume, while others might prefer anonymous validators with positive track records.

### What are the responsibilities of a validator?

Validators have two main responsibilities:

* **Be able to constantly run a correct version of the software:**Validators need to ensure that they are running an uncompromised and updated version of the software continuously.
* **Actively participate in governance:** Validators are required to vote on every proposal.

Additionally, validators are expected to be active members of the community. They should always be up-to-date with the current state of the ecosystem and staying abreast of any information on FunctionX so that they can make necessary upgrades, and be quick to respond to any changes.

### What does 'participating in governance' entail?

Validators and delegators on the f(x)Core can vote on proposals to change operational parameters (such as the block gas limit), coordinate upgrades, or make a decision on any given matter.

Validators play a special role in the governance system. Being the pillars of the system, they are required to vote on every proposal. It is especially important since validators will vote on behalf of the delegators if the delegators do not vote.

### What does staking imply?

Staking FX can be thought of as a safety deposit for validation activities. When a validator or a delegator wants to retrieve part or all of their deposit, they can send an `unbonding` transaction. Delegators to this validator who unbond their delegation must wait the duration of the UnbondingTime, a **3 weeks unbonding period**, during which time they are liable to being slashed for potential misbehaviors committed by the validator before the unbonding process started.

Validators, and their delegators, receive block rewards, fees, and have the right to participate in governance. If a validator misbehaves, a certain portion of their total stake is slashed. This means that every delegator that bonded their FX with this validator gets penalized in proportion to their bonded stake. Delegators are therefore incentivized to delegate to validators they believe are trustworthy.

### Can a validator run away with their delegators' FX?

In a DPoS blockchain, voting power is associated with the amount of FX one has. By delegating FX to a validator, a user delegates voting power. The more voting power a validator has, the more weight they have in the consensus and governance processes. This does not mean that the validator has custody of their delegators' FX. **By no means can a validator run away with its delegator's funds**.

Even though delegated funds cannot be stolen by their validators, delegators are still liable for their validators' misbehaviour.

### How often will a validator be chosen to propose the next block? Does it go up with the quantity of bonded FX?

The validator that is selected to propose the next block is called a proposer. Each proposer is selected deterministically, and the frequency of being chosen is proportional to the voting power (i.e. amount of bonded FX) of the validator. For example, if the total bonded stake across all validators is 100 FX and a validator's total stake is 10 FX, then this validator will proposer `~10% `of the blocks.

## Incentives

### What is the incentive to stake?

There are essentailly 2 different types of revenue:

* **Block rewards:** Native tokens of applications run by validators (e.g. FX on the f(x)Core) are inflated to produce block provisions. These provisions exist to incentivize FX holders to bond their stake, as non-bonded FX will be diluted over time.
* **Transaction fees:** The f(x)Core maintains a whitelist of token that are accepted as fee payment. The initial fee token is the `FX`.

This total revenue is divided among validators' staking pools according to each validator's weight. Then, within each validator's staking pool the revenue is divided among delegators in proportion to each delegator's stake. A commission on delegators' revenue is applied by the validator before it is distributed.

### What is the incentive to run a validator?

Validators earn proportionally more revenue than their delegators because of commissions.

Validators also play a major role in governance. If a delegator does not vote, they inherit the vote from their validator. This gives validators a major responsibility in the ecosystem.

### What are validators commission?

Revenue received by a validator's pool is split between the validator and their delegators. The validator can apply a commission on the part of the revenue that goes to their delegators. This commission is set as a percentage. Each validator is free to set their initial commission, maximum daily commission change rate and maximum commission. The f(x)Core enforces the parameter that each validator sets. Only the commission rate can change after the validator is created.

### How are block rewards distributed?

Block rewards are distributed proportionally to all validators relative to their voting power. This means that even though each validator gains FX with each reward, all validators will maintain equal weight over time.

Let us take an example where we have 10 validators with equal voting power and a commission rate of 1%. Let us also assume that the reward for a block is 1000 FX and that each validator has 20% of self-bonded FX. These tokens do not go directly to the proposer. Instead, they are evenly spread among validators. So now each validator's pool has 100 FX. These 100 FX will be distributed according to each participant's stake:

* Commission: `100*80%*1% = 0.8 FX`
* Validator gets: `100\*20% + Commission = 20.8 FX`
* All delegators get: `100\*80% - Commission = 79.2 FX`

Then, each delegator can claim their part of the 79.2 FX in proportion to their stake in the validator's staking pool.

### How are fees distributed?

Fees are similarly distributed with the exception that the block proposer can get a bonus on the fees of the block they propose if they include more than the strict minimum of required precommits.

When a validator is selected to propose the next block, they must include at least 2/3 precommits of the previous block. However, there is an incentive to include more than 2/3 precommits in the form of a bonus. The bonus is linear: it ranges from 1% if the proposer includes 2/3rd precommits (minimum for the block to be valid) to 5% if the proposer includes 100% precommits. Of course the proposer should not wait too long or other validators may timeout and move on to the next proposer. As such, validators have to find a balance between wait-time to get the most signatures and risk of losing out on proposing the next block. This mechanism aims to incentivize non-empty block proposals, better networking between validators as well as to mitigate censorship.

Let's take a concrete example to illustrate the aforementioned concept. In this example, there are 10 validators with equal stake. Each of them applies a 1% commission rate and has 20% of self-delegated FX. Now comes a successful block that collects a total of 4020 FX in fees.

First, a 25% tax is applied. The corresponding FX go to the reserve pool. Reserve pool's funds can be allocated through governance to fund bounties and upgrades.

* `25% * 4020 = 1005` FX go to the reserve pool.

1005 FX now remain. Let's assume that the proposer included 100% of the signatures in its block. It thus obtains the full bonus of 5%.

We have to solve this simple equation to find the reward R for each validator:

`9*R + R + R*5% = 1005 ⇔ R = 1005/10.05 = 100`

* For the proposer validator:
  * The pool obtains `R + R * 5%`: 105 FX
  * Commission: `105 * 80% * 1%` = 0.84 FX
  * Validator's reward: `105 * 20% + Commission` = 21.84 FX
  * Delegators' rewards: `105 * 80% - Commission` = 83.16 FX (each delegator will be able to claim its portion of these rewards in proportion to their stake)
* For each non-proposer validator:
  * The pool obtains R: 100 FX
  * Commission: `100 * 80% * 1%` = 0.8 FX
  * Validator's reward: `100 * 20% + Commission` = 20.8 FX
  * Delegators' rewards: `100 * 80% - Commission` = 79.2 FX (each delegator will be able to claim their portion of these rewards in proportion to their stake)

### What are the slashing conditions?

If a validator misbehaves, their delegated stake will be partially slashed. There are currently two faults that can result in slashing of funds for a validator and their delegators:

* **Double signing:** If someone reports on chain A that a validator signed two blocks at the same height on chain A and chain B, and if chain A and chain B share a common ancestor, then this validator will get slashed by 5% on chain A.
* **Downtime:** If a validator misses more than 95% of the last 20000 blocks, they will get slashed by 0.1%.

### Do validators need to self-delegate FX?

Yes, they do need to self-delegate at least `100FX`. Even though there is no obligation for validators to self-delegate more than `100FX`, delegators should want their validator to have more self-delegated FX in their staking pool. In other words, validators should have skin in the game.

In order for delegators to have some guarantee about how much skin-in-the-game their validator has, the latter can signal a minimum amount of self-delegated FX. If a validator's self-delegation goes below the limit that it predefined, this validator and all of its delegators will unbond.

### How to prevent concentration of stake in the hands of a few top validators?

For now the community is expected to behave in a smart and self-preserving way. When a mining pool in Bitcoin gets too much mining power the community usually stops contributing to that pool. The f(x)Core will rely on the same effect initially. Other mechanisms are in place to smoothen this process as much as possible:

* **Penalty-free re-delegation:** This is to allow delegators to easily switch from one validator to another, in order to reduce validator stickiness.
* **UI warning:** Wallets can implement warnings that will be displayed to users if they want to delegate to a validator that already has a significant amount of staking power.

## Technical Requirements

### What are hardware requirements?

Validators should expect to provision one or more data center locations with redundant power, networking, firewalls, HSMs and servers.

We expect that a modest level of hardware specifications will be needed initially and that they might rise as network use increases. Participating in the testnet is the best way to learn more.

### What are software requirements?

In addition to running a f(x)Core node, validators should develop monitoring, alerting and management solutions.

### What are bandwidth requirements?

The Cosmos network has the capacity for very high throughput relative to chains like Ethereum or Bitcoin.

We recommend that the data center nodes only connect to trusted full-nodes in the cloud or other validators that know each other socially. This relieves the data center node from the burden of mitigating denial-of-service attacks.

Ultimately, as the network becomes more heavily used, multigigabyte per day bandwidth is very realistic.

### What does running a validator imply in terms of logistics?

A successful validator operation will require the efforts of multiple highly skilled individuals and continuous operational attention. This will be considerably more involved than running a bitcoin miner for instance.

### How to handle key management?

Validators should expect to run an HSM that supports ed25519 keys. Here are potential options:

* YubiHSM 2
* Ledger Nano S
* Ledger BOLOS SGX enclave
* Thales nShield support

The Tendermint team does not recommend one solution above the other. The community is encouraged to bolster the effort to improve HSMs and the security of key management.

### What can validators expect in terms of operations?

Running effective operation is the key to avoiding unexpectedly unbonding or being slashed. This includes being able to respond to attacks, outages, as well as to maintain security and isolation in your data center.

### What are the maintenance requirements?

Validators should expect to perform regular software updates to accommodate upgrades and bug fixes. There will inevitably be issues with the network early in its bootstrapping phase that will require substantial vigilance.

### How can validators protect themselves from denial-of-service attacks?

Denial-of-service attacks occur when an attacker sends a flood of internet traffic to an IP address to prevent the server at the IP address from connecting to the internet.

An attacker scans the network, tries to learn the IP address of various validator nodes and disconnect them from communication by flooding them with traffic.

One recommended way to mitigate these risks is for validators to carefully structure their network topology in a so-called sentry node architecture.

Validator nodes should only connect to full-nodes they trust because they operate them themselves or are run by other validators they know socially. A validator node will typically run in a data center. Most data centers provide direct links the networks of major cloud providers. The validator can use those links to connect to sentry nodes in the cloud. This shifts the burden of denial-of-service from the validator's node directly to its sentry nodes, and may require new sentry nodes be spun up or activated to mitigate attacks on existing ones.

Sentry nodes can be quickly spun up or change their IP addresses. Because the links to the sentry nodes are in private IP space, an internet based attacked cannot disturb them directly. This will ensure validator block proposals and votes always make it to the rest of the network.

It is expected that good operating procedures on that part of validators will completely mitigate these threats.
