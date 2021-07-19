# f(x)Core developers documentation

## f(x)Core summary

* Each block size is 1048576Byte（1M）
* There is no gas limit for each block
* The consensus algorithm of f(x) Core is pBFT 
* The sequence of processing the validation requests are following the sequential order of receiving the valid requests (by validator nodes)
* ChainID is fxcore
* The current block time is 5 seconds
* The currency in f(x)Core is $FX, the initial supply of $FX is 378,604,525.462891.FX has 18 decimal points
* $FX can be delegated in f(x)Core
* Total circulating supply of $FX = Delegated asset $FX + Non-delegated asset $FX 
    * Delegated asset $FX = Total $FX that delegated in f(x)Core validator node
    * Non-delegated asset $FX = Ethereum cross chain locked fund  + Unclaimed reward of validator (including commission and transaction fee) + Unclaimed reward of delegator + Wallet balance + Pool of ecosystem and community + Locked fund of Governance
    * Ethereum cross chain locked fund = Total $FX (ERC20) on Ethereum


## The calculation of transaction fee

* GasPrice: Every validator node can set up their GasPrice limit, now the setting of GasPrice limit of every validator node is the same

* The computation of GasLimit is related to the size of transaction(s), number of reading of the transaction, number of writing of the transaction and transaction signing. The signing of the transaction is fixed at the moment.

    * Size of transaction GasLimit: Transaction bytes * 10

    * The computation of GasLimit of reading data: number of reading * cost of reading

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
    * Computation of GasLimit  of transaction signing:：

        * Fixed cost of ED25519: 590
        * Fixed cost of Secp256k1: 1000

* Transaction fee = GasPrice * GasLimit

> Conclusion: The transaction fee might be varied due to the different transaction logic (block size and numbers of writing and reading)  even though there are similar or homogenous transaction

##### 普Analysis of the GasLimit of normal transfer

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
* memo: transaction remarks, maximum 256 bytes, normally it leaves blank.

> Estimation: The current network GasLimit setting can process up to 80,000 normal transfer transactions

## Delegation 

Undelegation period: 21 days

Jail / discharge: The validator node that offline or failure to sign <5% of the block in the previous 20,000 blocks will be jail; validator node can apply for discharge after 10 minutes

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
