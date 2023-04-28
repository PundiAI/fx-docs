# Delegators FAQ

## What is a delegator?

People who cannot or do not want to operate [validator nodes](../validators/validator-overview.md) can still participate in the staking process as delegators. Active validators are chosen and ranked based on their total overall FX staked (this is the sum of their self-delegated stake as well as their delegators' stake). They are by no means solely chosen by their self-delegated stake. This is an important property and acts as a safeguard against validators that are bad actors. In a decentralized ecosystem, if a validator is a bad actor, delegators will undelegate their FX from them to avoid incurring any slashings of their delegated stake. Good actors will invariably have more FX delegated to them. Game theory dictates that over time, validators who are bad actors will exit the active validator set whilst good actors will remain or climb higher up the ranks, eventually resulting in a stable and secure ecosystem.

**Delegators share the revenue of their validators, but they also share the risks.** In terms of revenue, validators and delegators differ in that validators can apply a commission on the revenue that goes to their delegator before it is distributed. This commission is known to delegators beforehand and can only be changed according to predefined constraints (see [section](delegators-faq.md#choosing-a-validator) below).

In terms of risks, delegators' FX will be slashed if their validator misbehaves. For more, see [Risks](delegators-faq.md#risks) section.

To become delegators, FX holders need to send a "Delegate transaction" where they specify the amount of FX they want to stake and with which validator. A list of validators will be displayed in f(x)Core explorers ([mainnet](https://explorer.functionx.io/fxcore/validators)/[testnet](https://dhobyghaut-explorer.functionx.io)). Subsequently, if a delegator wants to unbond part or all of their stake, they will need to send an "Unbond transaction". Delegators will have to wait for 3 weeks to retrieve their FX after sending an "Unbond Transaction". Delegators can also send a "Redelegate Transaction" to switch from one validator to another, without having to go through the 3 weeks waiting period.

{% hint style="info" %}
However,there is a limit to how frequent you can redelegate. For more information on [redelegation](delegators-faq.md#redelegation).
{% endhint %}

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

There are currently two conditions that can result in slashing of funds for a validator and their delegators:

* **Double signing:** If someone reports on chain A that a validator signed two blocks at the same height on chain A and chain B, and if chain A and chain B share a common ancestor, then this validator will get slashed by 5% on chain A. Validators who double sign will be jailed and CANNOT be unjailed thereafter.
* **Downtime:** If a validator misses more than 95% of the last 20000 blocks (\~27.7hours), they will get slashed by 0.1%. Validators may `unjail` their validators after a 600s (10minute) window.

{% hint style="info" %}
The portion of FX that is subjected to slashing conditions is the total delegated FX. The rewards earned will not be subjected to slashing conditions.

If a validator is jailed, the same rules apply to redelegation and unbonding. For unbonding, you still have to wait 21 days. While for redelegation, you may do so and the [following rules](delegators-faq.md#redelegation) will apply.
{% endhint %}

### Redelegation

You can easily re-allocate your stake from one validator to another without having to wait 21 days to unbond. However, there's a limit or catch to this relegation feature.

**21 Day Cooldown: Remember this recurring number**

When a user requests to `undelegate` from a validator, the amount of `FX` that was requested for undelegation will be locked in **unbonding** state for 21 days. For simplicity, we call this the **21 day cooldown**. After the 21 day cooldown passes, a user will be able to make transactions with the `FX` that was previously in **unbonding** state. This cooldown also applies to certain scenarios in redelegation. In order to **redelegate a portion of delegated FX from Validator A → Validator B,** there are two options a user could choose from.

{% hint style="info" %}
Do note that during the unboding state, users will cease to earn rewards but will be subjected to slashing conditions if their validators misbehave.
{% endhint %}

#### Option #1 <a href="#9704" id="9704"></a>

Undelegate from **Validator A** and wait for the 21 day unbonding period (cooldown) to pass. Then, delegate the FX to **Validator B**.

* Taking this path may seem un-wise because you’ll have to wait 21 days to delegate that stake with another validator. This is where the **redelegation** feature comes in handy.

#### Option #2 <a href="#5320" id="5320"></a>

Use the redelegation feature to immediately redelegate the FX from **Validator A → Validator B.**

* This **redelegation** feature seems wonderful. You no longer have to wait 21 days to unbond and then delegate that stake to another validator. **But there’s a catch.**

#### The 7 Stacks Rule <a href="#39ce" id="39ce"></a>

Redelegating from **Validator A → Validator B** using the **same wallet.**

Let’s say you want to **redelegate** your FX from Validator A → Validator B using Wallet #1. With the same wallet address, you are only able to **redelegate from Validator A → Validator B up to 7 times in a 21 day period.**

#### Serial Redelegation (Validator Hopping) <a href="#cc75" id="cc75"></a>

Redelegating from **Validator A → Validator B**, then redelegating from **Validator B → Validator C** consecutively.

**Serial redelegation** is a rule that most delegators would be baffled by when they first try redelegating without looking into the rules. Here’s why.

* Redelegating from **Validator A → Validator B**, then consecutively redelegating from **Validator B → Validator C** does not work.
* **The 21 day cooldown applies to serial redelegation.**

Once you redelegate from Validator A → Validator B, **you will not be able to redelegate from Validator B to another validator for the next 21 days.**

In other words, **the validator on the receiving end of redelegation will be on a 21-day redelegation lock** (You will still be able to undelegate from or make some additional delegations to this validator. You just won’t be able to redelegate from the validator.).

_**You can’t consecutively do validator-hopping!**_&#x20;

Find more information on redelegation [here](https://medium.com/cosmostation/what-you-need-to-know-about-cosmos-atom-redelegation-e45ca7da6fdf).
