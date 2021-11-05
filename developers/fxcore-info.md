# f(x)Core developers documentation

## f(x)Core summary

* Each block size is 1048576Byte（1M）
* There is no gas limit for each block
* The consensus algorithm of f(x)Core is pBFT 
* The sequence of processing the validation requests are following the sequential order of receiving the valid requests (by validator nodes)
* ChainID is `fxcore`
* The current block time is 5 seconds
* The currency in f(x)Core is $FX, the initial supply of $FX is 378,604,525.462891.FX has 18 decimal points
* $FX can be delegated in f(x)Core
* Total circulating supply of $FX = Delegated asset $FX + Non-delegated asset $FX 
    * Delegated asset $FX = Total $FX that delegated in f(x)Core validator node
    * Non-delegated asset $FX = Ethereum cross chain locked fund  + Unclaimed reward of validator (including commission and transaction fee) + Unclaimed reward of delegator + Wallet balance + Pool of ecosystem and community + Locked fund of Governance
    * Ethereum cross chain locked fund = Total $FX (ERC20) on Ethereum


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

##### Analysis of the GasLimit of normal transfer

| Balance of the address | Amount of transfer | Is the receipt address a new address? | Fill in the FeePayer’section?  | Fill in the memo?  | Fill in the expire blockheight?  | GasPrice | Size of the transaction (bytes) | Consumption of gas |
| ---------------- | -------- | -------------------- | ---------------- | ------------ | ------------------------ | -------- | -------------- | ------- |
| 1*10^26          | 1        | Y                   | N               | N           | N                       | 0        | 290            | 50992   |
| 1*10^59          | 1        | Y                   | N               | N           | N                       | 0        | 290            | 52084   |
| 1*10^59          | 1*10^59  | Y                   | N               | N           | N                       | 0        | 350            | 51934   |
| 1*10^59          | 1*10^59  | N                   | N               | N           | N                       | 0        | 350            | 59085   |
| 1*10^59          | 1*10^59  | N                   | Y               | N           | N                       | 0        | 394            | 59525   |
| 1*10^59          | 1*10^59  | N                   | Y               | Y           | N                       | 0        | 653            | 62115   |
| 1*10^59          | 1*10^59  | N                   | Y               | Y           | Y                       | 0        | 664            | 62225   |
| 1*10^59          | 1*10^59  | N                   | Y               | Y           | Y                       | 1*10^12  | 691            | 77239   |
| 1*10^59          | 1*10^59  | N                   | Y               | Y           | Y                       | 1*10^18  | 696            | 77479   |
| 1*10^59          | 1*10^59  | N                   | Y               | Y           | Y                       | 1*10^26  | 704            | 77799   |

Remarks:

* New address refers to the address that exists / appears on the chain first time
* FeePayer: Inputting the wallet address to pay the transaction fee, blank means sender pays the transaction fee
* Memo: transaction remarks, maximum 256 bytes, normally it is left blank.

> Estimation: The current network GasLimit setting can process up to 80,000 normal transfer transactions

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

## Governance (GOV)）

* **Initiate a proposal**：A minimum of $FX is required to initiate a proposal.
* **Voting**：Validator will cast the vote on behalf of the delegators that delegated their vote ($FX) to that validator. Delegators cannot cast the vote themselves. Vote(s) ($FX) without delegation is invalid.

#### Governance Particulars

* Software update, docker upgrade
* Allocation and distribution of the community and ecosystem pool
* IBC local host and app upgrade
* Alteration of module and data
* Other

#### Voting rule

* Proposal shall be rejected if there are insufficient participated votes
* Proposal shall be rejected if there is none voting power
* Proposal shall be rejected, if there is over ⅔ voting power fail to cast the vote
* Proposal shall be rejected, if there is over ⅓ voting power reject the proposal
* Proposal shall be approved, if there is over ½  participated voting power approve the proposal
* Proposal shall be rejected, if there is over ½  participated voting power reject the proposal


## Ethereum cross chain 

Illustration of f(x) Core and Ethereum cross chain 

#### From Ethereum to f(x)Core

1. Users shall call the function of  ‘sendToFx’ of f(x)Bridge to make a transfer from Ethererum to f(x)Core, the event of ‘sendToFx‘ shall be broadcasted on chain after the verification.
2. The validator node(s) on f(x)Core shall monitor the event of ‘sendToFx‘ on Ethereum and sign and verify the event, thereafter, send a ‘MsgDepositClaim’ transaction to notify the event and signature to f(x)Core.
3. The cross chain transfer shall be confirmed after receiving the verification of ‘MsgDepositClaim’ from ⅔ of the validator nodes (on f(x)Core) 


#### From f(x)Core to Ethereum

1. Users shall make a cross chain withdrawal request on f(x)Core
2. One of the validator nodes on f(x)Core shall monitor the cross chain withdrawal request on f(x)Core and it shall execute the batch cross chain (MsgRequestBatch) order under one of following scenarios:
   1. 10 minutes after the execution of last cross chain batch order;
   2. The value of BridgeFee (USD value) is greater than the cross chain gas fee on Ethereum

    > `The estimation gas fee of cross chain withdrawal request (USD) = gas limit * gas price *eth Price` 
    > `Bridge Fee = The number of $FX * $FX price`

   | The number of validators | The number of withdrawal request | GasLimit (extreme) | GasLimit (normal) | GasLimit (average) | Gas ratio of average and normal |
   | ---------- | -------- | ----------------- | ---------------- | ----------------- | ---------------- |
   | 20         | 10       | 479631            | 288395           | 384013            | 1.33             |
   | 20         | 20       | 783022            | 417823           | 600422            | 1.44             |
   | 20         | 30       | 1090340           | 547742           | 819041            | 1.50             |
   | 20         | 40       | 1390788           | 677352           | 1034070           | 1.53             |
   | 20         | 50       | 1695490           | 807141           | 1251315           | 1.55             |
   | 20         | 60       | 2000185           | 936675           | 1468430           | 1.57             |
   | 20         | 70       | 2304848           | 1071161          | 1688004           | 1.58             |
   | 20         | 80       | 2607903           | 1202013          | 1904958           | 1.58             |
   | 20         | 90       | 2914709           | 1327786          | 2121247           | 1.60             |
   | 20         | 100      | 3190053           | 1453354          | 2321703           | 1.60             |

3. Validator nodes shall sign and verify the batch request (MsgConfirmBatch)
4. f(x)Core shall send the transaction (submint batch) to Ethereum to complete the cross chain withdrawal transaction(s).
