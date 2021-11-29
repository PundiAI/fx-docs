# f(x)Core developers documentation

## The calculation of transaction fee

* GasPrice: Every validator node can set their GasPrice limit, now the setting of GasPrice limit of every validator node is the same

* The computation of GasLimit is related to the size of transaction(s), number of transaction readings, number of transaction writings and transaction signing. The signing of the transaction is fixed at the moment.

    * Size of transaction GasLimit: Transaction bytes * 10

    * The computation of GasLimit of reading data: number of readings * cost of reading

      * ```
        HasCost:          1000 // to verify if cost needed
        ReadCostFlat:     1000 // to verify the cost of reading 
        ReadCostPerByte:  3 // cost of reading per byte
        IterNextCostFlat: 30 // total cost of reading
        ```

    * The computation of GasLimit of writing data: number of writing * cost of writing

      * ```
        WriteCostFlat:    2000 // cost of writing
        WriteCostPerByte: 30 // cost of writing per byte
        ```
    * Computation of GasLimit  of transaction signing:

        * Fixed cost of ED25519: 590
        * Fixed cost of Secp256k1: 1000

* Transaction fee = GasPrice * GasLimit

> Conclusion: The transaction fee may vary due to different transaction logic (block size and numbers of readings and writings)  even though the transactions are similar or homogenous

## Mint module

| Variables                     | Description                                                         | Value                   |
| ------------------------ | ------------------------------------------------------------ | -------------------- |
| Minter inflation variables |                                                              |                      |
| Inflation                | Initial annual inflation rate (computation on block basis, the inflation rate of each block are different)                | 0.35                 |
| AnnualProvisions         | Annual provision（computation on block basis,, the inflation rate of each block are different）                       | 0                    |
|                          |                                                              |                      |
| Params chain variables         |                                                              |                      |
| MintDenom                | Name of the newly minted token（changable）                           | FX                   |
| InflationRateChange      | Annual inflation change rate                                    | 0.3 (30%)            |
| InflationMax             | The maximum annual inflation rate                                              | 0.416762 (41.6762%)  |
| InflationMin             | The minimum annual inflation rate                                            | 0.17 (17%)           |
| GoalBonded               | Increment or decrement of the annual inflation rate  rule：staking ratio（ delegated Token / Total circulating token supply, if current bonded ratio less than GoalBonded, annual inflation rate will increase, until InflationMax; if current bonded ratio = GoalBonded, the inflation rate shall remain; if current bonded ratio higher than GoalBonded, the annual inflation rate will decrease, until InflationMin | 0.51 (51%)           |
| BlocksPerYear            | Estimated annual newly created block number formula：60 * 60 * 8766 / 5(average per block in 5 sec) | 6,311,520 (5 sec/block) |

* Inflation rate will be computed before each block is being created, formula as follows：

    1. Request (call) for Minter value & Params variables
    2. Request total bonded token in delegation
    3. Request BondedRatio = Total valide delegated token / Total circulating token supply
    4. Computing the inflation rate for next block (Annual inflation rate / 6,311,520)

    This will require Params variable and BondedRatio

    Formula：Latest inflation rate = Current inflation rate + ( (1 - Delegated ratio/ GoalBonded) * InflationRateChange ) / BlocksPerYear

    minter.Inflation cannot exceed maximum value（Params.InflationMax）and minimum value （Params.InflationMin）

    * Computing the next annual inflation provision 

    Variables needed：Latest inflation rate and Total circulation token supply

    Formula：minter.AnnualProvisions = Latest inflation rate * Total circulation token supply

* Computing the number of newly generated/minted token on the next block

    Variables required：Latest inflation provision and estimated annual number of newly minted block 
    
    Number of newly minted token =  Latest inflation provision   / Estimated annual number of newly minted block
    
    1. Sending the number of token that required to be minted to Mint module address
    2. Sending transaction fee from Fee module address to Mint module address (transaction fee)

    Example：
    
    Assume the variables as follows：
    
    - Total circulating supply： 378,604,525.462891
    - The number of valid delegated token：20,000,000
    - Block time: 5 sec / block
    - Initial annual inflation rate： 0.35
    - Annual InflationRateChange ： 0.3
    
    Valid BondedRatio = 20,000,000/378,604,525.462891 = 0.05282557036
    
    Latest inflation rate minter. Inflation = minter.Inflation + ( (1 - valid delagation rate / GoalBonded) *  InflationRateChange)
    
    The inflation rate of next block：
    
    0.35 + ( (1 - 0.05282557036/0.51) * 0.3) / 6,311,520 = 0.3500000426
    
    The provision of inflation rate：
    
    0.3500000426 * 378,604,525.462891 = 132,511,600.04056464
    
    The number of newly Mint Token on next block：
    
    132,511,600.04056464 / 6,311,520  = 20.9951960923

* The currency in f(x)Core is $FX, the initial supply of $FX is 378,604,525.462891.FX has 18 decimal points
* Total circulating supply of $FX = Delegated asset $FX + Non-delegated asset $FX 
    * Delegated asset $FX = Total $FX that delegated in f(x)Core validator node
    * Non-delegated asset $FX = Ethereum cross chain locked fund  + Unclaimed reward of validator (including commission and transaction fee) + Unclaimed reward of delegator + Wallet balance + Pool of ecosystem and community + Locked fund of Governance
    * Ethereum cross chain locked fund = Total $FX (ERC20) on Ethereum

## Delegation/bonding module

| Variable               | Description                                                         | f(x)            |
| ------------------ | ------------------------------------------------------------ | --------------- |
| unbonding_time     | Undelegated period / period of withdrawing delegated token                                          | 1814400s ( approx 21 days) |
| max_validators     | The maximum number of validator node                                            | 20 (changable through governance voting) |
| max_entries        | Maximum number of delegating / undelegating transaction on each block (it can only transact 7 delegate/undelegate transactions concurrently  | 7               |
| historical_entries | The number of snapshot of block that would be stored (each block will store the snapshot of validator node, for IBC module| 20000           |
| bond_denom         | Token that can be delegated                                                     | FX              |

## Distribution module

Undelegation period: 21 days

Jail / discharge: The validator node that offline or failure to sign <5% of the block in the previous 20,000 blocks will be jail; validator node can apply for discharge after 10 minutes

| Variable                 | Description                                                       | f(x)      |
| --------------------- | ---------------------------------------------------------- | --------- |
| community_tax         | Block reward that belongs to the pool of community and ecosystem                                  | 0.4 (40%) |
| base_proposer_reward  | Base proposal block reward for every valid validator node                                   | 0.01 (1%) |
| bonus_proposer_reward | Current block proposer reward for the specific block proposer                                       | 0.04 (4%) |
| withdraw_addr_enabled | Enabling the setting of block reward request and collection address (The block reward will automatically send to the designated wallet address once the block reward withdrawal request has been made | true      |

* Block reward and distribution mechanic
  
    Block reward = Total block verification reward + Transaction fee
  
    Assume the following: Voting rate is 100% (all valid validator node participates the block verification voting); Total block verification reward per block is 20 FX; and the block reward can be further divided into 3 parts:
    
    * Part 1： The proposal block reward for block proposer：
    
        Formula: Total block verification reward * ( base proposal block reward + current block proposer reward * (current voting power of the proposing validator node / total validator voting power) 
        
        The proposal block reward::  20 * (0.01 + 0.04 ) * 1 = 1FX  
        
        The remaining balance of block verification reward per block： 20 - 1 = 19FX
    
    * Part 2：Block verification reward of all valid validator node
    
        The verification block reward that belongs to all validator nodes:
        
        Formula: 1 - Block proposer reward portion - Community and ecosystem pool
        
        Block verification reward: 1 - 0.05 - 0.4 = 0.55
        
        Block verification reward of each validator node:
        
        Formula: Total block verification reward * Block verification reward belongs to validator node * Voting power of the validator node 
        
        Assume the voting power of validator node A is 50%:
        
        Block verification reward of validator node A: 20 * 0.55 * 0.5 =5.5FX
        
        Assume the voting power of all validator nodse: 100%:
        
        Block verification reward belongs to all validator nodes: 20 * 0.55 * 1 = 11FX
    
    * Part 3: Community and ecosystem pool
    
        Formula: Total block verification reward - The proposal block reward (part 1) - Block reward for validator node (part 2) 
        
        Block verification reward belongs to community and ecosystem pool: 20 -1 -11 = 8FX
    
* The distribution of block reward between validator and delegator

    Delegators need to pay commission fee (if any) to the validator that he/she delegates token to 
    
    Assume: The remaining balance of block reward available to delegator is 2FX; Commission rate is 1%
    
    * The commission fee of validator:
    
        Formula: The remaining balance of block reward available to delegator * Commission rate
        
        Commission: 2 * 0.01 = 0.02
    
    * The block reward of delegator:
    
        Formula: The remaining balance of block reward available to validator and delegator - Commission collected
        
        Block reward after commission：2 - 0.02 = 1.98
    
* APY computation of delegator

    The total return of the specific validator and the delegator (that delegates to the specific validator) = Block reward (including inflation and transaction fee) * (1 - Community and ecosystem pool) * The voting power % of that specific validator node
    
    Return of delegator = (The total return of the specific validator and the delegator (that delegates to the specific validator) * (1- commission rate) / principal 

## Governance (GOV)

* **Initiate a proposal**: A minimum of $FX is required to initiate a proposal.
* **Voting**: Validator will cast the vote on behalf of the delegators that delegated their vote ($FX) to that validator, if delegators did not vote. Vote(s) ($FX) without delegation is invalid.

#### Governance Particulars

* Software update
* Allocation and distribution of the community and ecosystem pool
* IBC local host and app upgrade
* Alteration of module and data
* Support Cross-chain
* Other

#### Voting rule

* Proposal shall be rejected if there are insufficient participated votes
* Proposal shall be rejected if there is none voting power
* Proposal shall be rejected, if there is over ⅔ voting power fail to cast the vote
* Proposal shall be rejected, if there is over ⅓ voting power reject the proposal
* Proposal shall be approved, if there is over ½  participated voting power approve the proposal
* Proposal shall be rejected, if there is over ½  participated voting power reject the proposal
