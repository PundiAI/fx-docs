---
description: >-
  This page contains explanation and examples of the governance proposal process
  on f(x)Core.
---

# Governance Proposal Information

### Governance Proposal Types

Currently there are the following governance proposal types:

| Type                         | Description                                                                                                                          |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Ecosystem Genesis Fund (EGF) | Proposal for request of funds from the EGF, for the complete process please read [this](https://github.com/FunctionX/FunctionX-EGF). |
| Add Cross Chain Parameters   | Proposal for adding cross-chain functionality of a new chain and configuring the parameters of Relayers.                             |
| Upgrade Chain Oracles        | Proposal for updating the configuration of Relayers on the newly added chain.                                                        |
| Parameter Change             | Proposal for configuring f(x)core chain parameters.                                                                                  |
| Others                       | Any governance proposal that does not involved a modification of the source code.                                                    |

### Process Flow of Governance Proposal

![](<../.gitbook/assets/image (7).png>)

### Initiate a Proposal

This section will give details and information on initiating a proposal.

It is strongly encouraged that a proposal be discussed on the [forum](https://forum.functionx.io/) before initiating it.

{% hint style="info" %}
For the EGF proposal, please fork the repository [here](https://github.com/FunctionX/FunctionX-EGF), create a copy of the application template and follow the steps as stated in the repository.
{% endhint %}

#### Deposit

A deposit is required by the proposer to initiate a proposal. It is designed to prevent spam.

To submit a proposal, the proposer can head to the [f(x)Core block explorer](https://explorer.functionx.io/fxcore/proposals/form), connect their address and fill up the form. It requires an `initial deposit` of at least 1,000 FX. The `initial deposit` will be _counted as part of the entire deposit_ of initiating the governance voting.

In order to initiate the governance voting (enter the `voting period`), all proposals are required to have a minimum amount of FX deposit, referred to as the _deposit threshold_ or `min deposit`. The `min deposit` is currently set at 10,000 FX. This parameter can be changed via governance.

The proposal owners are not required to deposit the full amount on their own. Once a proposal is submitted successfully, the proposal will enter the `max deposit period` or the _deposit period_ where other FX holders can increase the proposal's deposit by sending a Deposit transaction. The `max deposit period` is currently set as 14 days.

Once the `min deposit` of 10,000 FX is reached, the proposal will automatically enter the `voting period`.

#### Examples

Tom initiates a proposal with 1,002 FX, proposal enters `max deposit period`. Dick deposits 8,000 FX to the same proposal 2 days later. The total deposit is now 9,002 FX. Harry then deposits 1,000 FX a few days later. After Harry's deposit, `min deposit` has been reached and the proposal enters the `voting period`.

Tommy initiates a proposal with 10,001 FX, proposal immediately enters the `voting period` as the `min deposit` has been reached.

### Voting Period

The `voting period` is currently set as 14 days. Once a proposal has entered the `voting period`, a qualified FX holder will have the right to vote on the proposal.

A qualified FX holder is as follow:

* the person is a Delegator to (at least) one active Validator on f(x)Core before the proposal entered the `voting period`
* the person is an active Validator on f(x)Core before the proposal entered the `voting period`

The above FX holders are considered to have bonded their FX tokens. Unbonded FX holders and other users do not have the right to participate in governance. However, they can submit and deposit on proposals.

#### Quorum (%)

`quorum` is defined as the 'minimum percentage of total staked (bonded) that needs to cast a vote for proposal to be valid'.

`quorum` is set at 40% and is calculated as:

$$
Voters' bonded FX / Total bonded FX * 100
$$

If a proposal fails to reach quorum, the proposal will be marked as 'Rejected' regardless of the results of the vote.

If a proposal reached quorum, the proposal will be marked as 'Passed' and the results of the vote will be accepted.

#### Examples

Total bonded FX is 1,000,000 FX, only 390,000 bonded FX participated during the `voting period`. Quorum is 39%. Since it did not reach quorum after 14 days, the proposal will be marked as 'Rejected' and the deposit will be burned.

Total bonded FX is 1,000,000 FX, only 400,100 bonded FX participated during the `voting period`. Quorum is 40.01%. Since it reached quorum after 14 days, the proposal is valid and will be marked passed.

#### Voting

Participants cast their votes by selecting from the option set that consists of 4 options:

* YES
* NO
* NoWithVeto
* Abstain

"NoWithVeto" counts as "No" but also adds a Veto vote.

"Abstain" counts towards the participation to make up quorum. It allows voters to signal that they do not intend to vote in favour or against the proposal but accept the result of the vote.

Participants with bonded tokens can vote either by connecting their wallets on the [explorer](https://explorer.functionx.io/fxcore/proposals), or through the f(x)wallet.

#### Inheritance

If a delegator does not vote, it will inherit its validator vote, except:

* If the validator votes before its validator, it will not inherit from the validator's vote
* If the delegator votes after its validator, it will override its validator vote with its own.

Basically, if a delegator votes, the bonded tokens the delegator owns will contribute to whatever its vote is. If a delegator does not vote, and its validator votes, their bonded tokens will contribute to the vote, regardless of what the delegator wanted. Therefore, it is strongly encouraged for delegators to vote.

### Results

The validity of a proposal and its results is determined by the following conditions:

* `quorum` is reached or not
* `threshold` is reached or not (minimum proportion of Yes votes for proposal to pass)
* `veto threshold` is reached or not (Minimum value of Veto votes to total votes ratio for proposal to be vetoed)

`quorum` is 40%. `threshold` is 50%. `veto threshold` is 33.4%

| Voting Results                                                                                                                                                                                                       | Proposal Status |  Deposit |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------: | :------: |
| <ul><li>Failed to reach <code>quorum</code></li></ul>                                                                                                                                                                |     Rejected    |  Burned  |
| <ul><li><code>quorum</code> reached</li><li>ratio of 'YES' votes to total votes exceed <code>threshold</code></li><li>ratio of 'NoWithVeto' votes to total votes less than <code>veto threshold</code></li></ul>     |      Passed     | Refunded |
| <ul><li><code>quorum</code> reached</li><li>ratio of 'NO' votes to total votes exceed <code>threshold</code></li><li>ratio of 'NoWithVeto' votes to total votes less than <code>veto threshold</code></li></ul>      |     Rejected    | Refunded |
| <ul><li><code>quorum</code> reached</li><li>ratio of 'Abstain' votes to total votes exceed <code>threshold</code></li><li>ratio of 'NoWithVeto' votes to total votes less than <code>veto threshold</code></li></ul> |     Rejected    | Refunded |
| <ul><li><code>quorum</code> reached</li><li>ratio of 'NoWithVeto' votes to total votes more than <code>veto threshold</code></li><li>in this case, ratio of 'YES', 'NO', 'Abstain' votes does not matter </li></ul>  |     Rejected    |  Burned  |

#### Deposit Burns

| When                                               | Details                                                                                                                                                  |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| the Deposit Period ends                            | When the submitted proposal fails to reach the deposit threshold at the end of the Deposit Period, the proposal's deposits will be burned                |
| the proposal is not passed after the voting period | When the proposal fails to reach the quorum at the end of the voting period, the proposal will be marked as 'Rejected' and its deposit will be be burned |
| the proposal get vetoed                            | when the proposal is vetoed, its deposit will be burned                                                                                                  |

#### Summary

![Graphical summary](<../.gitbook/assets/infrographic for gov proposal info (1).png>)
