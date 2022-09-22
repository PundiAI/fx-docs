---
description: The various parameters of modules in f(x)Core and transaction fee calculation
---

# f(x)Core Info

## The calculation of transaction fee

#### Transaction fee = GasPrice \* GasLimit

* **GasPrice**:
  * Every validator node can set their GasPrice, currently the GasPrice of every validator node is the same.
* **GasLimit:**
  * Gas limit (gas units) refers to the maximum amount of gas users are willing to consume on a transaction.
  * The computation of GasLimit is related to the size of transaction(s), number of transaction readings, writings and signings. The number of transaction signings is fixed at the moment.
    * Size of transaction GasLimit: Transaction bytes \* 10
    * The computation of GasLimit of reading data: number of readings \* cost of reading
      * ```
        HasCost:          1000 // to verify if cost needed
        ReadCostFlat:     1000 // to verify the cost of reading 
        ReadCostPerByte:  3 // cost of reading per byte
        IterNextCostFlat: 30 // total cost of reading
        ```
    * The computation of GasLimit of writing data: number of writing \* cost of writing
      * ```
        WriteCostFlat:    2000 // cost of writing
        WriteCostPerByte: 30 // cost of writing per byte
        ```
    * Computation of GasLimit of transaction signing:
      * Fixed cost of ED25519: 590
      * Fixed cost of Secp256k1: 1000

> Conclusion: Even though the transactions are similar or homogenous, transaction fees may vary due to the difference in transaction logic (block size, numbers of readings and writings).&#x20;

## Mint module

| Variables                  | Description                                                                                                                                                                                                                                                                                                                                                                                                          | Value                   |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| Minter inflation variables |                                                                                                                                                                                                                                                                                                                                                                                                                      |                         |
| Inflation                  | Initial annual inflation rate (computation on block basis, the inflation rate of each block are different)                                                                                                                                                                                                                                                                                                           | 0.35                    |
| AnnualProvisions           | Annual provision (computation on block basis, the inflation rate of each block are different)                                                                                                                                                                                                                                                                                                                        | 0                       |
|                            |                                                                                                                                                                                                                                                                                                                                                                                                                      |                         |
| Params chain variables     |                                                                                                                                                                                                                                                                                                                                                                                                                      |                         |
| MintDenom                  | Name of the newly minted token (dependent on sub-chains)                                                                                                                                                                                                                                                                                                                                                             | FX                      |
| InflationRateChange        | Annual inflation change rate                                                                                                                                                                                                                                                                                                                                                                                         | 0.3 (30%)               |
| InflationMax               | The maximum annual inflation rate                                                                                                                                                                                                                                                                                                                                                                                    | 0.416762 (41.6762%)     |
| InflationMin               | The minimum annual inflation rate                                                                                                                                                                                                                                                                                                                                                                                    | 0.17 (17%)              |
| GoalBonded                 | Increment or decrement of the annual inflation rate rule: staking ratio (delegated Token / Total circulating token supply, if current bonded ratio less than GoalBonded, annual inflation rate will increase, until InflationMax; if current bonded ratio = GoalBonded, the inflation rate shall remain; if current bonded ratio higher than GoalBonded, the annual inflation rate will decrease, until InflationMin | 0.51 (51%)              |
| BlocksPerYear              | Estimated annual newly created block number formula: 60 \* 60 \* 8766 / 5(average per block in 5 sec)                                                                                                                                                                                                                                                                                                                | 6,311,520 (5 sec/block) |

*   Inflation rate will be computed before each block is being created, formula as follows:

    1. Request (call) for Minter value & Params variables
    2. Request total bonded token in delegation
    3. Request BondedRatio = Total delegated token / Total circulating token supply
    4. Computing the inflation rate for next block (Annual inflation rate / 6,311,520)

    This will require Params variable and BondedRatio

    Formula: Latest inflation rate = Current inflation rate + ( (1 - Delegated ratio/ GoalBonded) \* InflationRateChange ) / BlocksPerYear

    minter.Inflation cannot exceed maximum value (Params.InflationMax) and minimum value (Params.InflationMin)

    * Computing the next annual inflation provision

    Variables needed: Latest inflation rate and Total circulation token supply

    Formula: minter.AnnualProvisions = Latest inflation rate \* Total circulation token supply
*   Computing the number of newly generated/minted token on the next block

    Variables required: Latest inflation provision and estimated annual number of newly minted block

    Number of newly minted token = Latest inflation provision / Estimated annual number of newly minted block

    1. Sending the number of token that required to be minted to Mint module address
    2. Sending transaction fee from Fee module address to Mint module address (transaction fee)

    Example:

    Assume the variables as follows:

    * Total circulating supply: 378,604,525.462891
    * The number of valid delegated token: 20,000,000
    * Block time: 5 sec / block
    * Initial annual inflation rate: 0.35
    * Annual InflationRateChange: 0.3

    Valid BondedRatio = 20,000,000/378,604,525.462891 = 0.05282557036

    Latest inflation rate minter. Inflation = minter.Inflation + ( (1 - valid delagation rate / GoalBonded) \* InflationRateChange)

    The inflation rate of next block:

    0.35 + ( (1 - 0.05282557036/0.51) \* 0.3) / 6,311,520 = 0.3500000426

    The provision of inflation rate:

    0.3500000426 \* 378,604,525.462891 = 132,511,600.04056464

    The number of newly Mint Token on next block:

    132,511,600.04056464 / 6,311,520 = 20.9951960923
* The currency in f(x)Core is $FX, the initial supply of $FX is 378,604,525.462891.FX has 18 decimal points
* Total circulating supply of $FX = Delegated asset $FX + Non-delegated asset $FX
  * Delegated asset $FX = Total $FX that delegated in f(x)Core validator node
  * Non-delegated asset $FX = Ethereum cross chain locked fund + Unclaimed reward of validator (including commission and transaction fee) + Unclaimed reward of delegator + Wallet balance + Pool of ecosystem and community + Locked fund of Governance
  * Ethereum cross chain locked fund = Total $FX (ERC20) on Ethereum

## Delegation/bonding module

| Variable            | Description                                                                                                                                 | f(x)                                     |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| unbonding\_time     | Undelegated period / period of withdrawing delegated token                                                                                  | 1814400s ( approx 21 days)               |
| max\_validators     | The maximum number of validator node                                                                                                        | 20 (changable through governance voting) |
| max\_entries        | Maximum number of delegating / undelegating transaction on each block (it can only transact 7 delegate/undelegate transactions concurrently | 7                                        |
| historical\_entries | The number of snapshot of block that would be stored (each block will store the snapshot of validator node, for IBC module                  | 20000                                    |
| bond\_denom         | Token that can be delegated                                                                                                                 | FX                                       |

## Distribution module

Undelegation period: 21 days

Jail / discharge: The validator node that offline or failure to sign <5% of the block in the previous 20,000 blocks will be jail; validator node can apply for discharge after 10 minutes

| Variable                | Description                                                                                                                                                                                           | f(x)      |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| community\_tax          | Block reward that belongs to the pool of community and ecosystem                                                                                                                                      | 0.4 (40%) |
| base\_proposer\_reward  | Base proposal block reward for every valid validator node                                                                                                                                             | 0.01 (1%) |
| bonus\_proposer\_reward | Current block proposer reward for the specific block proposer                                                                                                                                         | 0.04 (4%) |
| withdraw\_addr\_enabled | Enabling the setting of block reward request and collection address (The block reward will automatically send to the designated wallet address once the block reward withdrawal request has been made | true      |

*   Block reward and distribution mechanic

    Block reward = Total block verification reward + Transaction fee

    Assume the following: Voting rate is 100% (all valid validator node participates the block verification voting); Total block verification reward per block is 20 FX; and the block reward can be further divided into 3 parts:

    *   Part 1: The proposal block reward for block proposer:

        Formula: Total block verification reward \* ( base proposal block reward + current block proposer reward \* (current voting power of the proposing validator node / total validator voting power)

        The proposal block reward: 20 \* (0.01 + 0.04 ) \* 1 = 1FX

        The remaining balance of block verification reward per block: 20 - 1 = 19FX
    *   Part 2: Block verification reward of all valid validator node

        The verification block reward that belongs to all validator nodes:

        Formula: 1 - Block proposer reward portion - Community and ecosystem pool

        Block verification reward: 1 - 0.05 - 0.4 = 0.55

        Block verification reward of each validator node:

        Formula: Total block verification reward \* Block verification reward belongs to validator node \* Voting power of the validator node

        Assume the voting power of validator node A is 50%:

        Block verification reward of validator node A: 20 \* 0.55 \* 0.5 =5.5FX

        Assume the voting power of all validator nodse: 100%:

        Block verification reward belongs to all validator nodes: 20 \* 0.55 \* 1 = 11FX
    *   Part 3: Community and ecosystem pool

        Formula: Total block verification reward - The proposal block reward (part 1) - Block reward for validator node (part 2)

        Block verification reward belongs to community and ecosystem pool: 20 -1 -11 = 8FX
*   The distribution of block reward between validator and delegator

    Delegators need to pay commission fee (if any) to the validator that he/she delegates token to

    Assume: The remaining balance of block reward available to delegator is 2FX; Commission rate is 1%

    *   The commission fee of validator:

        Formula: The remaining balance of block reward available to delegator \* Commission rate

        Commission: 2 \* 0.01 = 0.02
    *   The block reward of delegator:

        Formula: The remaining balance of block reward available to validator and delegator - Commission collected

        Block reward after commission: 2 - 0.02 = 1.98
*   APY computation of delegator

    The total return of the specific validator and the delegator (that delegates to the specific validator) = Block reward (including inflation and transaction fee) \* (1 - Community and ecosystem pool) \* The voting power % of that specific validator node

    Return of delegator = (The total return of the specific validator and the delegator (that delegates to the specific validator) \* (1- commission rate) / principal

## Governance module (gov)

The gov module allows on-chain governance system. A brief explanation of the process flow of submitting a proposal is as follow (for an in-depth explanation and examples, go [here](../governance/governance-proposal-information/)):&#x20;

1. To submit a proposal, it requires an _initial deposit_ of at least 1000 FX, there is a deposit period (`max deposit period`) for the `min deposit` to be reached
2. The `min deposit` is 10000 FX (the _initial deposit_ is counted towards the threshold)
3. `voting period` starts when the 10000 FX threshold is reached, and will last for 14 days
4. `quorum` must be reached for the proposal to be valid, which means at least 40% of bonded FX have to participate in voting
5. If 50% or more of the participants (weighted by FX) voted YES, the proposal has reached the `threshold` and is considered 'PASS'
6. If 50% or more of the participants (weighted by FX) voted no, the proposal has not reached the `threshold` and is considered 'REJECTED'
7. If 33.4% (`veto threshold`) or more of the participants (weighted by FX) voted 'NoWithVeto', irregardless of the % of YES, the proposal is considered 'REJECTED'

The parameters are as follow:

| Variable           | Description                                                                | Value                   |
| ------------------ | -------------------------------------------------------------------------- | ----------------------- |
| voting period      | Time period for voting (nanoseconds)                                       | 1209600000000000        |
| quorum             | Minimum percentage of total staked (bonded) for proposal to be valid       | 0.4                     |
| threshold          | Minimum proportion of Yes votes for proposal to pass                       | 0.5                     |
| veto threshold     | Minimum value of Veto votes to total votes ratio for proposal to be vetoed | 0.334                   |
| min deposit        | Minimum deposit for a proposal to enter voting period (FX, 18 decimals)    | 10000000000000000000000 |
| max deposit period | Maximum time period for FX holders to deposit on a proposal (nanoseconds)  | 1209600000000000        |
