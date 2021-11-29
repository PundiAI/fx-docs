# Delegators FAQ

## What is a delegator?

People who cannot or do not want to operate [validator nodes](../validators/validator-overview.md) can still participate in the staking process as delegators. Active validators are chosen and ranked based on their total overall FX staked (this is the sum of their self-delegated stake as well as their delegators' stake). They are by no means solely chosen by their self-delegated stake. This is an important property and acts as a safeguard against validators that are bad actors. In a decentralized ecosystem, if a validator is a bad actor, delegators will undelegate their FX from them to avoid incurring any slashings of their delegated stake. Good actors will invariably have more FX delegated to them. Game theory dictates that over time, validators who are bad actors will exit the active validator set whilst good actors will remain or climb higher up the ranks, eventually resulting in a stable and secure ecosystem.

**Delegators share the revenue of their validators, but they also share the risks.** In terms of revenue, validators and delegators differ in that validators can apply a commission on the revenue that goes to their delegator before it is distributed. This commission is known to delegators beforehand and can only be changed according to predefined constraints (see [section](delegators-faq.md#choosing-a-validator) below).

In terms of risks, delegators' FX will be slashed if their validator misbehaves. For more, see [Risks](delegators-faq.md#risks) section.

To become delegators, FX holders need to send a "Delegate transaction" where they specify the amount of FX they want to stake and with which validator. A list of validators will be displayed in f(x)Core explorers ([mainnet](https://explorer.functionx.io/fxcore/validators)/[testnet](https://dhobyghaut-explorer.functionx.io)). Subsequently, if a delegator wants to unbond part or all of their stake, they will need to send an "Unbond transaction". Delegators will have to wait for 3 weeks to retrieve their FX after sending an "Unbond Transaction". Delegators can also send a "Redelegate Transaction" to switch from one validator to another, without having to go through the 3 weeks waiting period.

For a technical guide on how to become a delegator, click [here](delegator-cli-guide.md).

## Choosing a validator

In order to choose their validators, delegators have access to a range of information directly in f(x)Core block explorers ([mainnet](https://explorer.functionx.io/fxcore/validators)/[testnet](https://dhobyghaut-explorer.functionx.io)).

* **Validator's moniker**: Name of the validator.
* **Validator's description**: Description provided by the validator's operator.
* **Validator's website**: Link to the validator's website.
* **Initial commission rate**: The commission rate on revenue charged to any delegator by the validator (see below for more detail).
* **Commission max change rate:** The maximum daily increase of the validator's commission. This parameter cannot be changed by the validator operator.
* **Maximum commission:** The maximum commission rate this validator candidate can charge. This parameter cannot be changed by the validator operator.
* **Minimum self-bond amount**: Minimum amount of FX the validator candidate need to have bonded at all time. If the validator's self-bonded stake falls below this limit, their entire staking pool (i.e. all its delegators) will unbond. This parameter exists as a safeguard for delegators. Indeed, when a validator misbehaves, part of their total stake gets slashed. This included the validator's self-delegateds stake as well as their delegators' stake. Thus, a validator with a high amount of self-delegated FX has more skin-in-the-game than a validator with a low amount. The minimum self-bonded parameter guarantees delegators that a validator will never fall below a certain amount of self-bonded stake, thereby ensuring a minimum level of skin-in-the-game. This parameter can only be increased by the validator operator.

## Directives of delegators

Being a delegator is not a passive task. Here are the main directives of a delegator:

* **Perform careful due diligence on validators before delegating.** If a validator misbehaves, part of their total stake, which includes the stake of their delegators, will be slashed. Delegators should therefore select validators carefully and to their own research on who they are delegating to.
* **Actively monitor their validator after having delegated.** Delegators should ensure that the validators they delegate to does not misbehave, meaning that they have good uptime, do not double sign or get compromised, and participate in governance. They should also monitor the commission rate that is being applied to the revenue they are receiving. If a delegator is not satisfied with its validator, they can unbond or rebond and switch to another validator (Note: Delegators do not have to wait for the unbonding period to switch validators. Rebonding takes effect immediately).
* **Participate in governance.** Delegators can and are expected to participate actively in governance. A delegator's voting power is proportional to the size of their bonded stake. If a delegator does not vote, they will inherit the vote of their validator(s). If they do vote, they override the vote of their validator(s).

## Revenue

Validators and delegators earn revenue in exchange for their services. This revenue is given in three forms:

* **Block provisions (FX):** They are paid in newly created FX. Block provisions exist to incentivize FX holders to stake. The yearly inflation rate is calculated to target 2/3 bonded stake. If the total bonded stake in the network is less than 2/3 of the total FX supply, inflation increases until it reaches 41%. If the total bonded stake is more than 2/3 of the FX supply, inflation decreases until it reaches 17%. This means that if total bonded stake stays less than 2/3 of the total FX supply for a prolong period of time, unbonded FX holders can expect their FX value to deflate in value by 41% (compounded) per year.
* **Transaction fees (various tokens):** Each transfer on the f(x)Core comes with transactions fees. These fees can be paid in any currency that is whitelisted by the FunctionX governance. Fees are distributed to bonded FX delegators in proportion to their stake. The first whitelisted token at launch is FX.

## Validator Commission

Each validator receives revenue based on their total stake. Before this revenue is distributed to delegators, the validator can apply a commission. In other words, delegators have to pay a commission to their validators on the revenue they earn. Let us look at a concrete example:

We consider a scenario where there are 10 validators in the ecosystem and all have an equal weight. So any one validator's proportion of total stake in the entire ecosystem (i.e. self-delegated stake + delegated stake) is 10%. Take an example of one particular validator that has 20% self-delegated stake and applies a commission of 10%. Now let us consider a block with the following revenue:

* 990 FX in block provisions
* 10 FX in transaction fees.

This amounts to a total of 1000 FX to be distributed among all staking pools. Each validator's staking pool will receive 10% of this total amount which amounts to 100FX.

Now let us look at the internal distribution of revenue:

* Commission = `10% * 80% * 100` FX = 8 FX
* Validator's revenue = `20% * 100` FX + Commission = 28 FX
* Delegators' total revenue = `80% * 100` FX - Commission = 72 FX

Then, each delegator in the staking pool can claim their portion of the delegators' total revenue.

## Risks

Staking FX is not a risk-free activity. First, staked FX are locked up, and retrieving them requires a 3 week waiting period called unbonding period. Additionally, if a validator misbehaves, a portion of their total stake can be slashed. This includes the stake of their delegators.

There is one main slashing condition:

* **Double signing:** If someone reports that a validator signed two different blocks with the same chain ID at the same height, this validator will get slashed.

This is why FX holders should perform their due diligence on validators before delegating. It is also important that delegators actively monitor the activity of their validators. If a validator behaves suspiciously or is too often offline, delegators can choose to unbond from them or switch to another validator. **Delegators can also mitigate risk by distributing their stake across multiple validators.**
