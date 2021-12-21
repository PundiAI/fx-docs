# f(x)cored Commands Documentation

## f(x)Core Daemon

`fxcored` is the tool that enables you to interact with the node that runs on the `f(x)Core network`. In order to install it, follow the [installation procedure](../f-x-core/installation.md).

### Main structure of running fxcored commands

> The `fxcored` help commands are nested. So `$ fxcored` will output docs for the top level commands (status, config, query, and tx). You can access documentation for sub commands with further help commands.

The very first command to generate a list of available commands:

```
fxcored
```

Return:

```
FunctionX Core Chain App

Usage:the 
  fxcored [command]

Available Commands:
  add-genesis-account Add a genesis account to genesis.json
  collect-gentxs      Collect genesis txs and output a genesis.json file
  config              Update or query an application configuration file
  debug               Tool for helping with debugging your application
  export              Export state to JSON
  gentx               Generate a genesis tx carrying a self delegation
  help                Help about any command
  init                Initialize private validator, p2p, genesis, and application configuration files
  keys                Manage your application's keys
  migrate             Migrate genesis to a specified target version
  query               Querying subcommands
  start               Run the full node
  status              Query remote node for status
  tendermint          Tendermint subcommands
  testnet             Initialize files for a fxchain testnet
  tx                  Transactions subcommands
  unsafe-reset-all    Resets the blockchain database, removes address book files, and resets data/priv_validator_state.json to the genesis state
  validate-genesis    validates the genesis file at the default location or at the location passed as an arg
  version             Print the application binary version information

Flags:
  -h, --help                 help for fxcored
      --home string          directory for config and data (default "/root/.fxcore")
      --log_filter strings   The logging filter can discard custom log type (ABCIQuery) (default "")
      --log_format string    The logging format (json|plain) (default "plain")
      --log_level string     The logging level (trace|debug|info|warn|error|fatal|panic) (default "info")
      --trace                print out full stack trace on errors
```

The return value will include:

* a header which explains what the command is, for example `FunctionX Core Chain App`
* the usage for example `fxcored [command]` where you will need to input fxcored and a follow up command like `fxcored tx`
* all available commands
* and all flags which might be needed for commands

We will be going through a common command along with the various sub commands and flags. Selecting the `tx` command:

```
fxcored tx
```

Return:

```
Transactions subcommands

Usage:
  fxcored tx [flags]
  fxcored tx [command]

Available Commands:
  bank                Bank transaction subcommands
  broadcast           Broadcast transactions generated offline

...

Additional help topics:
  fxcored tx upgrade     Upgrade transaction subcommands
```

You may either choose to insert a `flag` or a `command` after `fxcored tx`:

```
fxcored tx --help

or

fxcored tx gov
```

the latter will return:

```
Governance transactions subcommands

Usage:
  fxcored tx gov [flags]
  fxcored tx gov [command]

Available Commands:
  deposit         Deposit tokens for an active proposal
  submit-proposal Submit a proposal along with an initial deposit
  vote            Vote for an active proposal, options: yes/no/no_with_veto/abstain

...
Use "fxcored tx gov [command] --help" for more information about a command.
```

continuing:

```
fxcored tx gov submit-proposal
```

return:

```
Error: invalid message: can't proto marshal <nil>
Usage:
  fxcored tx gov submit-proposal [flags]
  fxcored tx gov submit-proposal [command]

Available Commands:
  cancel-software-upgrade Cancel the current software upgrade proposal
  community-pool-spend    Submit a community pool spend proposal
  param-change            Submit a parameter change proposal
  software-upgrade        Submit a software upgrade proposal
...

Use "fxcored tx gov submit-proposal [command] --help" for more information about a command.
```

now if you use the `--help` flag:

```
fxcored tx gov submit-proposal --help
```

return:

```
Submit a proposal along with an initial deposit.
Proposal title, description, type and deposit can be given directly or through a proposal JSON file.

Example:
$ fxcored tx gov submit-proposal --proposal="path/to/proposal.json" --from mykey

Where proposal.json contains:

{
  "title": "Test Proposal",
  "description": "My awesome proposal",
  "type": "Text",
  "deposit": "10test"
}

Which is equivalent to:

$ fxcored tx gov submit-proposal --title="Test Proposal" --description="My awesome proposal" --type="Text" --deposit="10test" --from mykey

Usage:
  fxcored tx gov submit-proposal [flags]
  fxcored tx gov submit-proposal [command]

Available Commands:
  cancel-software-upgrade Cancel the current software upgrade proposal
  community-pool-spend    Submit a community pool spend proposal
  param-change            Submit a parameter change proposal
  software-upgrade        Submit a software upgrade proposal

...
```

following the prompt:

{% hint style="info" %}
In this case, a transaction fee must be paid! So do remember to add --fees XXXXXFX in your command. If you did not input the --fees flag, you will be prompted to add --fees 1200000000000000000FX in your command.

The minimum fee for this transaction is 1.2FX which after multiplying by 10^18 is 1200000000000000000FX.

Alternatively, a more universal command is --gas=auto --gas-adjustment=1.25 --gas-prices=4000000000000FX. --gas=auto automatically assesses the gas used for that transaction. This depends on the transaction itself and also the state of the blockchain. --gas-adjustment=1.25 means that there will be a 25% buffer added to the automatically assessed gas amount. --gas-prices=4000000000000FX is the gas price you will be paying for. For more details on gas, kindly refer to the section on gas below.

The **gas-prices** flag will default to the gas price value of your current node if it is not specified.
{% endhint %}

```
fxcored tx gov submit-proposal --title="BLINDGOTCHI" --description="CLAUDIOXBARROS’s pet" --type="Text" --deposit=" 200000000000000000000FX"  --gas=auto --gas-adjustment=1.25 --gas-prices=4000000000000FX --from richard
```

return:

```
{"body":{"messages":[{"@type":"/cosmos.gov.v1beta1.MsgSubmitProposal","content":{"@type":"/cosmos.gov.v1beta1.TextProposal","title":"BLINDGOTCHI","description":"CLAUDIOXBARROS’s pet"},"initial_deposit":[{"denom":"FX","amount":"200000000000000000000"}],"proposer":"fx124n6hpxkkn3r9j3tzwzhax2rd5s3crwq7p94fd"}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[{"denom":"FX","amount":"1200000000000000000"}],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]:
```

after inputting y:

```
{"height":"1632790","txhash":"C25C5A00D7EEEE5E3B7B0557320AE7F1D33992C8ADBFB2539C1F369F2B494725","codespace":"","code":0,"data":"0A160A0F7375626D69745F70726F706F73616C120308A001","raw_log":"[{\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"submit_proposal\"},{\"key\":\"sender\",\"value\":\"fx124n6hpxkkn3r9j3tzwzhax2rd5s3crwq7p94fd\"},{\"key\":\"module\",\"value\":\"governance\"},{\"key\":\"sender\",\"value\":\"fx124n6hpxkkn3r9j3tzwzhax2rd5s3crwq7p94fd\"}]},{\"type\":\"proposal_deposit\",\"attributes\":[{\"key\":\"amount\",\"value\":\"200000000000000000000FX\"},{\"key\":\"proposal_id\",\"value\":\"160\"}]},{\"type\":\"submit_proposal\",\"attributes\":[{\"key\":\"proposal_id\",\"value\":\"160\"},{\"key\":\"proposal_type\",\"value\":\"Text\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"fx10d07y265gmmuvt4z0w9aw880jnsr700jqjzsmz\"},{\"key\":\"sender\",\"value\":\"fx124n6hpxkkn3r9j3tzwzhax2rd5s3crwq7p94fd\"},{\"key\":\"amount\",\"value\":\"200000000000000000000FX\"}]}]}]","logs":[{"msg_index":0,"log":"","events":[{"type":"message","attributes":[{"key":"action","value":"submit_proposal"},{"key":"sender","value":"fx124n6hpxkkn3r9j3tzwzhax2rd5s3crwq7p94fd"},{"key":"module","value":"governance"},{"key":"sender","value":"fx124n6hpxkkn3r9j3tzwzhax2rd5s3crwq7p94fd"}]},{"type":"proposal_deposit","attributes":[{"key":"amount","value":"200000000000000000000FX"},{"key":"proposal_id","value":"160"}]},{"type":"submit_proposal","attributes":[{"key":"proposal_id","value":"160"},{"key":"proposal_type","value":"Text"}]},{"type":"transfer","attributes":[{"key":"recipient","value":"fx10d07y265gmmuvt4z0w9aw880jnsr700jqjzsmz"},{"key":"sender","value":"fx124n6hpxkkn3r9j3tzwzhax2rd5s3crwq7p94fd"},{"key":"amount","value":"200000000000000000000FX"}]}]}],"info":"","gas_wanted":"200000","gas_used":"91282","tx":null,"timestamp":""}
```

### Setting up fxcored

The main command used to set up `fxcored` is the following:

```bash
fxcored config <config file name> <key> [value] [flags]
```

It allows you to set a default value for each given flag.

First, set up the address of the full-node you want to connect to:

```bash
fxcored config <config file name> <host>:<port>

# example: fxcored config config.toml rpc.laddr https://77.87.106.33:26657
```

If you run your own full-node, just use `tcp://localhost:26657` as the address.

Then, let us set the default value of the `--trust-node` flag:

```bash
fxcored config config.toml trust-node true

# Set to true if you trust the full-node you are connecting to, false otherwise
```

Finally, let us set the `chain-id` of the blockchain we want to interact with:

```bash
fxcored config config.toml chain-id dhobyghaut
```

{% hint style="info" %}
for Mainnet the ChainId should be **fxcore**
{% endhint %}

### Keys

#### Key Types

There are three types of key representations that are used:

* `fx`
  * Derived from account keys generated by `fxcored keys add`
  * Used to receive funds
  * Addresses that only have a preceeding `fx` are wallet addresses
  * e.g. `fx15h6vd5f0wqps26zjlwrc6chah08ryu4hzzdwhc`
* `fxvaloper`
  * Used to associate a validator to it's operator
  * Used to invoke staking commands
  * Addresses preceeding with `fxvaloper`are validator consensus address
  * e.g. `fxvaloper1carzvgq3e6y3z5kz5y6gxp3wpy3qdrv928vyah`
* `fxpub`
  * Derived from account keys generated by `fxcored keys add`
  * Addresses that preceed with `fxpub` are the wallet public key
  * e.g. `fxpub1zcjduc3q7fu03jnlu2xpl75s2nkt7krm6grh4cc5aqth73v0zwmea25wj2hsqhlqzm`
* `fxvalconspub`
  * Generated when the node is created with `fxcored init`.
  * Get this value with `fxcored tendermint show-validator`
  * Addresses preceeding `fxvalconspub` are validator consensus public key
  * e.g. `fxvalconspub1zcjduepq0ms2738680y72v44tfyqm3c9ppduku8fs6sr73fx7m666sjztznqzp2emf`

#### Migrate Keys From Legacy On-Disk Keybase To OS Built-in Secret Store

Older versions of `fxcored` used store keys in the user's home directory. If you are migrating from an old version of `fxcored` you will need to migrate your old keys into your operating system's credentials storage by running the following command:

```bash
fxcored keys migrate <old_home_dir> [flags]
```

The command will prompt for each passphrase. If a passphrase is incorrect, it will skip the respective key.

#### Generate Keys

You'll need an account with a private and public key pair (a.k.a. `sk, pk` respectively) to be able to receive funds, send txs, bond tx, etc.

To generate a new _secp256k1_ key:

```bash
fxcored keys add <account_name>
```

The output of the above command will contain a _seed phrase_. It is recommended to save the _seed phrase_ in a safe place so that in case you forget the password of the operating system's credentials store, you could eventually regenerate the key from the seed phrase with the following command:

```bash
fxcored keys add <account_name> --recover
```

If you check your private keys, you'll now see `<account_name>`:

```bash
fxcored keys show <account_name>
```

Additionally and ⚠⚠ importantly, if you wish to have an added layer of protection on your keys, you may add the `--keyring-backend` flag and specify the file name. Setting your key up this way will ensure another layer of protection for signing any transactions.

> \--keyring-backend string Select keyring's backend (os|file|test) (default "test")

```
fxcored keys add <secondKeyName> \
  --ledger \
  --index <i> \
  --keyring-backend <file_name>
```

for example:

```
fxcored keys add shangqi \
  --ledger \
  --index 3 \
  --keyring-backend file
```

you will be prompted for a keyring passphrase (password must be at least 8 characters) :

```
Enter keyring passphrase:
```

In the future, whenever you use this account to sign off on a transaction, you will have to add the `--keyring-backend <file_name>` flag and enter the keyring passphrase.

{% hint style="info" %}
Save a backup of your keyring passphrase in a secure place. Losing your keyring passphrase will result in the lost of all your funds created using the keyring passphrase❗

Also to access your keys in the keyring file do not forget to add the --keyring flag
{% endhint %}

View the validator operator's address via:

```bash
fxcored keys show <account_name> --bech=val
```

You can see all your available keys by typing:

```bash
fxcored keys list
```

View the validator pubkey for your node by typing:

```bash
fxcored tendermint show-validator
```

{% hint style="info" %}
Note that this is the Tendermint signing key, _not_ the operator key you will use in delegation transactions. Warning: We strongly recommend _NOT_ using the same passphrase for multiple keys. The Function X team will not be responsible for the loss of funds.
{% endhint %}

#### Generate Multisig Public Keys

You can generate and print a multisig public key by typing:

```bash
fxcored keys add --multisig=name1,name2,name3[...] --multisig-threshold=K new_key_name
```

> Be sure to note that for multisig accounts, if you were to create any transaction, for example `--from=<multisig_account>` your `<multisig_account>` needs to be the wallet address ie `fx123l3kjltjwlfgjslfg....`. Only for those non-multisig accounts can you use the name of the account ie `--from=sheldoncooper`.

`K` is the minimum number of private keys that must have signed the transactions that carry the public key's address as signer.

The `--multisig` flag must contain the name of public keys that will be combined into a public key that will be generated and stored as `new_key_name` in the local database. All names supplied through `--multisig` must already exist in the local database. Unless the flag `--nosort` is set, the order in which the keys are supplied on the command line does not matter, i.e. the following commands generate two identical keys:

```bash
fxcored keys add --multisig=foo,bar,baz --multisig-threshold=2 multisig_address
fxcored keys add --multisig=baz,foo,bar --multisig-threshold=2 multisig_address
```

Multisig addresses can also be generated on-the-fly and printed through the which command:

```bash
fxcored keys show --multisig-threshold K name1 name2 name3 [...]
```

For more information regarding how to generate, sign and broadcast transactions with a multi signature account see [Multisig Transactions](f-x-cored-commands-documentation.md#multisig-transactions).

### Tx Broadcasting

When broadcasting transactions, `fxcored` accepts a `--broadcast-mode` flag. This flag can have a value of `sync` (default), `async`, or `block`, where `sync` makes the client return a CheckTx response, `async` makes the client return immediately, and `block` makes the client wait for the tx to be committed (or timing out).

It is important to note that the `block` mode should **not** be used in most circumstances. This is because broadcasting can timeout but the tx may still be included in a block. This can result in many undesirable situations. Therefore, it is best to use `sync` or `async` and query by tx hash to determine when the tx is included in a block.

### Fees & Gas

Each transaction may either use the --fees or --gas flags, but not both.

Validator's have a minimum gas price (multi-denom) configuration and they use this value when determining if they should include the transaction in a block during `CheckTx`, where `gasPrices >= minGasPrices`. Note, your transaction must use fees that are greater than or equal to **any** of the denominations the validator requires.

**Note**: With such a mechanism in place, validators may start to prioritize txs by `gasPrice` in the mempool, so providing higher fees or gas prices may yield higher tx priority.

e.g.

```bash
fxcored tx bank send ... --fees=4000000000000FX
```

or

```bash
fxcored tx bank send ... --gas-prices=4000000000000FX
```

To query the gas price of your current node:

```
fxcored query other gasPrice
```

{% hint style="info" %}
Note: You may want to cap the maximum gas that can be consumed by the transaction via the `--gas` flag. If you pass `--gas=auto`, the gas supply will be automatically estimated before executing the transaction. Gas estimate might be inaccurate as state changes could occur in between the end of the simulation and the actual execution of a transaction, thus an adjustment is applied on top of the original estimate in order to ensure the transaction is broadcasted successfully. The adjustment can be controlled via the `--gas-adjustment` flag. The default value is 1.0.
{% endhint %}

### Account

#### Get Tokens

On a testnet, getting tokens is usually done via a faucet. You may refer to this [link](fxtestnetfaucet.md).

#### Query Account Balance

After receiving tokens to your address, you can view your account's balance by typing:

```bash
fxcored q bank balances <account_fx>
```

{% hint style="info" %}
Note When you query an account balance with zero tokens, you will get this error: `No account with address <account_fx> was found in the state.` This can also happen if you fund the account before your node has fully synced with the chain. These are both normal.
{% endhint %}

### Send Tokens

The following command could be used to send coins from one account to another:

```bash
 fxcored tx bank send <from_key_or_address> <to_address> <amount>
```

{% hint style="info" %}
Note: The `amount` argument accepts the format `<value|coin_name>.` For example, 10000000000000000000FX which is equivalent to `10FX`
{% endhint %}

Now, view the updated balances of the origin and destination accounts:

```bash
fxcored query account <account_fx>
fxcored query account <destination_fx>
```

You can also check your balance at a given block by using the `--height` flag:

```bash
fxcored query account <account_fx> --height <int>
```

You can simulate a transaction without actually broadcasting it by appending the `--dry-run` flag to the command line (this is to return a gas estimate):

```bash
fxcored tx bank send <from_key_or_address> <to_address> <amount> --dry-run
```

Furthermore, you can build a transaction and print its JSON format to STDOUT by appending `--generate-only` to the list of the command line arguments. Running the following command will store build the transaction and store it in a file named "unsignedSendTx.json":

```bash
fxcored tx send <sender_address> <recipient_address> 10000000000000000000FX \
  --chain-id=<chain_id> \
  --generate-only > unsignedSendTx.json
```

```bash
fxcored tx sign \
  --chain-id=<chain_id> \
  --from=<key_name> \
  unsignedSendTx.json > signedSendTx.json
```

> The `--generate-only` flag prevents `fxcored` from accessing the local keybase. Thus when such flag is supplied `<sender_key_name_or_address>` must be an address.

You can validate the transaction's signatures by typing the following:

```bash
fxcored tx sign --validate-signatures signedSendTx.json
```

You can broadcast the signed transaction to a node by providing the JSON file to the following command:

```bash
fxcored tx broadcast <file_path>
eg.
fxcored tx broadcast signedSendTx.json
```

### Query Transactions

#### Matching a Set of Events

You can use the transaction search command to query for transactions that match a specific set of `events`, which are added on every transaction.

Each event is composed by a key-value pair in the form of `{eventType}.{eventAttribute}={value}`. Events can also be combined to query for a more specific result using the `&` symbol.

You can query transactions by `events` as follows:

```bash
fxcored query txs --events='message.sender=fx1...'
```

And for using multiple `events`:

```bash
fxcored query txs --events='message.sender=fx1...&message.action=withdraw_delegator_reward'
```

The pagination is supported as well via `page` and `limit`:

```bash
fxcored query txs --events='message.sender=fx1...' --page=1 --limit=20
```

> The action tag always equals the message type returned by the `Type()` function of the relevant message.
>
> You can find a list of available `events` on each of the SDK modules:
>
> * [Staking events](https://github.com/cosmos/cosmos-sdk/blob/master/x/staking/spec/07\_events.md)
> * [Governance events](https://github.com/cosmos/cosmos-sdk/blob/master/x/gov/spec/04\_events.md)
> * [Slashing events](https://github.com/cosmos/cosmos-sdk/blob/master/x/slashing/spec/06\_events.md)
> * [Distribution events](https://github.com/cosmos/cosmos-sdk/blob/master/x/distribution/spec/06\_events.md)
> * [Bank events](https://github.com/cosmos/cosmos-sdk/blob/master/x/bank/spec/04\_events.md)

#### Matching a Transaction's Hash

You can also query a single transaction by its hash using the following command:

```bash
fxcored query tx [hash]
```

{% hint style="info" %}
tx hash on the block explorer are preceded with 0x. please omit the 0x from the tx hash
{% endhint %}

### Slashing

#### Unjailing

To unjail your jailed validator

```bash
fxcored tx slashing unjail --from <validator-operator-addr>
```

#### Signing Info

To retrieve a validator's signing info:

```bash
fxcored query slashing signing-info <validator-pubkey>
```

#### Query Parameters

You can get the current slashing parameters via:

```bash
fxcored query slashing params
```

### Minting

You can query for the minting/inflation parameters via:

```bash
fxcored query mint params
```

To query for the current inflation value:

```bash
fxcored query mint inflation
```

To query for the current annual provisions value:

```bash
fxcored query mint annual-provisions
```

### Checking block information and validators signatures

The following command will query for a transaction by hash in a committed block:

```
fxcored query block-results
```

The followin command will get verified data for a the block at given height:

```
fxcored query block
```

{% hint style="info" %}
Using these commands and filtering out the necessary information, you will be able to deduce the uptime of other validators by checking if they missed any signatures for that block.
{% endhint %}

### Staking

#### Set up a Validator

Please refer to the [Validator Setup](../validators/validator-setup.md) section for a more complete guide on how to set up a validator.

#### Delegate to a Validator

On the upcoming mainnet, you can delegate `FX` to a validator. These [delegators](../delegators/delegators-faq.md) can receive part of the validator's fee revenue. Read more about the [incentives](../delegators/delegators-faq.md#revenue).

**Query Validators**

You can query the list of all validators of a specific chain:

```bash
fxcored query staking validators
```

If you want to get the information of a single validator you can check it with:

```bash
fxcored query staking validator <account_fxval>
```

#### Bond Tokens

On the f(x)Core mainnet, we delegate `FX`. Here's how you can bond tokens to a testnet validator (_i.e._ delegate):

```bash
fxcored tx staking delegate \
  <validator> \
  <amount> \
  --from=<key_name>
  --fees=<fees>
```

`<validator>` is the operator address of the validator to which you intend to delegate. If you are running a local testnet, you can find this with:

```bash
fxcored keys show [name] --bech val
```

where `[name]` is the name of the key you specified when you initialized `fxcored`.

While tokens are bonded, they are pooled with all the other bonded tokens in the network. Validators and delegators obtain a percentage of shares that equal their stake in this pool.

**Query Delegations**

{% hint style="info" %}
In this section and the next, do make sure you check if the command has a plural form or not. Adding an (s) behind delegation to delegations results in a different command.
{% endhint %}

Once submitted a delegation to a validator, you can see it's information by using the following command:

```bash
fxcored query staking delegation <delegator_addr> <validator_addr>
```

Or if you want to check all your current delegations with disctinct validators:

```bash
fxcored query staking delegations <delegator_addr>
```

You can also query all of the delegations to a particular validator:

```bash
fxcored query delegations-to <account_fxval>
```

#### Unbond Tokens

If for any reason the validator misbehaves, or you just want to unbond a certain amount of tokens, use this following command.

```bash
fxcored tx staking unbond \
  <validator_addr> \
  10FX \
  --from=<key_name> \
  --chain-id=<chain_id>
```

The unbonding will be automatically completed when the unbonding period has passed.

**Query Unbonding-Delegations**

Once you begin an unbonding-delegation, you can see it's information by using the following command:

```bash
fxcored query staking unbonding-delegation <delegator_addr> <validator_addr>
```

Or if you want to check all your current unbonding-delegations with disctinct validators:

```bash
fxcored query staking unbonding-delegations <account_fx>
```

Additionally, as you can get all the unbonding-delegations from a particular validator:

```bash
fxcored query staking unbonding-delegations-from <account_fxval>
```

#### Redelegate Tokens

A redelegation is a type delegation that allows you to bond illiquid tokens from one validator to another:

```bash
fxcored tx staking redelegate \
  <src-validator-operator-addr> \
  <dst-validator-operator-addr> \
  10FX \
  --from=<key_name> \
  --chain-id=<chain_id>
```

Here you can also redelegate a specific `shares-amount` or a `shares-fraction` with the corresponding flags.

The redelegation will be automatically completed when the unbonding period has passed.

**Query Redelegations**

Once you begin a redelegation, you can see it's information by using the following command:

```bash
fxcored query staking redelegation <delegator_addr> <src_val_addr> <dst_val_addr>
```

Or if you want to check all your current unbonding-delegations with distinct validators:

```bash
fxcored query staking redelegations <account_fx>
```

Additionally, as you can get all the outgoing redelegations from a particular validator:

```bash
fxcored query staking redelegations-from <account_fxval>
```

{% hint style="info" %}
However, there is a limit to how frequent you can redelegate. For more information on [redelegation](../delegators/delegators-faq.md#redelegation).
{% endhint %}

#### Query Parameters

Parameters define high level settings for staking. You can get the current values by using:

```bash
fxcored query staking params
```

With the above command you will get the values for:

* Unbonding time
* Maximum numbers of validators
* Coin denomination for staking

All these values will be subject to updates through a `governance` process by `ParameterChange` proposals.

#### Query Pool

A staking `Pool` defines the dynamic parameters of the current state. You can query them with the following command:

```bash
fxcored query staking pool
```

With the `pool` command you will get the values for:

* Bonded tokens
* Not-bonded tokens

### Governance

Governance is the process from which users in the f(x)Core can come to consensus on software upgrades, parameters of the mainnet or signaling mechanisms through text proposals. This is done through voting on proposals, which will be submitted by `FX` holders on the mainnet.

Some considerations about the voting process:

* Voting is done by bonded `FX` holders on a 100 bonded `FX` 1 vote basis
* Delegators who do not vote will inherit the vote of their validator
* Votes are tallied at the end of the voting period (2 weeks on mainnet). Addresses can vote multiple times before the end of the voting period to update their `Option` value (incurring transaction fees each time), only the most recently casted vote will count as valid
* Voters can choose between options `Yes`, `No`, `NoWithVeto` and `Abstain`
* By the end of the voting period, a proposal is accepted if:
  * `(YesVotes / (YesVotes+NoVotes+NoWithVetoVotes)) > 1/2`
  * `(NoWithVetoVotes / (YesVotes+NoVotes+NoWithVetoVotes)) < 1/3`
  * `((YesVotes+NoVotes+NoWithVetoVotes) / totalBondedStake) >= quorum`

For more information about the governance process and how it works, please check out the Governance module [specification](https://github.com/cosmos/cosmos-sdk/tree/master/x/gov/spec).

{% hint style="info" %}
The minimum deposit for a governance proposal is 10,000FX (through the command line, this amounts to 10000000000000000000000FX after multiplying by 10^18).
{% endhint %}

#### Create a Governance Proposal

In order to create a governance proposal, you must submit an initial deposit along with a title and description. Various modules outside of governance may implement their own proposal types and handlers (eg. parameter changes), where the governance module itself supports `Text` proposals. Any module outside of governance has it's command mounted on top of `submit-proposal`.

To submit a `Text` proposal (the title and description fields will be string input so enclose the input in ""):

```bash
fxcored tx gov submit-proposal \
  --title=<title> \
  --description=<description> \
  --type="Text" \
  --deposit="1000000FX" \
  --from=<name> \
  --chain-id=<chain_id>
```

You may also provide the proposal directly through the `--proposal` flag which points to a JSON file containing the proposal.

To submit a parameter change proposal, you must provide a proposal file as its contents are less friendly to CLI input:

```bash
fxcored tx gov submit-proposal param-change <path/to/proposal.json> \
  --from=<name> \
  --chain-id=<chain_id>
```

Where `proposal.json` contains the following:

```json
{
  "title": "Param Change",
  "description": "Update max validators",
  "changes": [
    {
      "subspace": "staking",
      "key": "MaxValidators",
      "value": 105
    }
  ],
  "deposit": [
    {
      "denom": "stake",
      "amount": "10000000"
    }
  ]
}
```

{% hint style="info" %}
Currently parameter changes are _evaluated_ but not _validated_, so it is very important that any `value` change is valid (ie. correct type and within bounds) for its respective parameter, eg. `MaxValidators` should be an integer and not a decimal.

Proper vetting of a parameter change proposal should prevent this from happening (no deposits should occur during the governance process), but it should be noted regardless.
{% endhint %}

> The `SoftwareUpgrade` command is currently not supported as it's not implemented and currently does not differ from the semantics of a `Text` proposal.

**Query Proposals**

Once created, you can now query information of the proposal:

```bash
fxcored query gov proposal <proposal_id>
```

Or query all available proposals:

```bash
fxcored query gov proposals
```

You can also query proposals filtered by `voter` or `depositor` by using the corresponding flags.

To query for the proposer of a given governance proposal:

```bash
fxcored query gov proposer <proposal_id>
```

#### Increase Deposit

In order for a proposal to be broadcasted to the network, the amount deposited must be above a `minDeposit` value (initial value: `10000000000000000000000FX`). If the proposal you previously created didn't meet this requirement, you can still increase the total amount deposited to activate it. Once the minimum deposit is reached, the proposal enters voting period:

```bash
fxcored tx gov deposit <proposal_id> "10000000000000000000000FX" \
  --from=<name> \
  --chain-id=<chain_id>
```

> _NOTE_: Proposals that don't meet this requirement will be deleted after `MaxDepositPeriod` is reached.

**Query Deposits**

Once a new proposal is created, you can query all the deposits submitted to it:

```bash
fxcored query gov deposits <proposal_id>
```

You can also query a deposit submitted by a specific address:

```bash
fxcored query gov deposit <proposal_id> <depositor_address>
```

#### Vote on a Proposal

After a proposal's deposit reaches the `MinDeposit` value, the voting period opens. Bonded `FX` holders can then cast vote on it:

```bash
fxcored tx gov vote <proposal_id> <Yes/No/NoWithVeto/Abstain> \
  --from=<name> \
  --chain-id=<chain_id>
```

**Query Votes**

Check the vote with the option you just submitted:

```bash
fxcored query gov vote <proposal_id> <voter_address>
```

You can also get all the previous votes submitted to the proposal with:

```bash
fxcored query gov votes <proposal_id>
```

#### Query proposal tally results

To check the current tally of a given proposal you can use the `tally` command:

```bash
fxcored query gov tally <proposal_id>
```

#### Query Governance Parameters

To check the current governance parameters run:

```bash
fxcored query gov params
```

To query subsets of the governance parameters run:

```bash
fxcored query gov param voting
fxcored query gov param tallying
fxcored query gov param deposit
```

### Fee Distribution

#### Query Distribution Parameters

To check the current distribution parameters, run:

```bash
fxcored query distribution params
```

#### Query distribution Community Pool

To query all coins in the community pool which is under Governance control:

```bash
fxcored query distribution community-pool
```

#### Query outstanding rewards

To check the current outstanding (un-withdrawn) rewards of a particular validator (transaction fees+block rewards), run:

```bash
fxcored query distribution validator-outstanding-rewards <validator-addr>
```

#### Query Validator Commission

To check the current outstanding commission for a validator (transaction fees), run:

```bash
fxcored query distribution commission <validator_address>
```

#### Query Validator Slashes

To check historical slashes for a validator, run:

```bash
fxcored query distribution slashes <validator_address> <start_height> <end_height>
```

#### Query Delegator Rewards

To check current rewards for a delegation (were they to be withdrawn), run:

```bash
fxcored query distribution rewards <delegator_address> <validator_address>
```

#### Query All Delegator Rewards

To check all current rewards for a delegation (were they to be withdrawn), run:

```bash
fxcored query distribution rewards <delegator_address>
```

### Claiming Rewards

#### Claiming rewards for delegators and validators

Withdraw rewards from a given delegation address, and optionally withdraw validator commission if the delegation address given is a validator operator:

```
fxcored tx distribution withdraw-rewards <validator-addr> --from <_name>
```

Withdraw the validator's commission in addition to the rewards:

```
fxcored tx distribution withdraw-rewards validator-addr> --from mykey --commission
```

### Multisig Transactions

Multisig transactions require signatures of multiple private keys. Thus, generating and signing a transaction from a multisig account involve cooperation among the parties involved. A multisig transaction can be initiated by any of the key holders, and at least one of them would need to import other parties' public keys into their Keybase and generate a multisig public key in order to finalize and broadcast the transaction.

For example, given a multisig key comprising the keys `p1`, `p2`, and `p3`, each of which is held by a distinct party, the user holding `p1` would require to import both `p2` and `p3` in order to generate the multisig account public key:

```bash
fxcored keys add \
  p2 \
  --pubkey=fxpub1addwnpepqtd28uwa0yxtwal5223qqr5aqf5y57tc7kk7z8qd4zplrdlk5ez5kdnlrj4

fxcored keys add \
  p3 \
  --pubkey=fxpub1addwnpepqgj04jpm9wrdml5qnss9kjxkmxzywuklnkj0g3a3f8l5wx9z4ennz84ym5t

fxcored keys add \
  --multisig=p1,p2,p3[...] \
  --multisig-threshold=2 \
  new_key_name
```

A new multisig public key `p1p2p3` has been stored, and its address will be used as signer of multisig transactions:

```bash
fxcored keys show --address p1p2p3
```

You may also view multisig threshold, pubkey constituents and respective weights by viewing the JSON output of the key or passing the `--show-multisig` flag:

```bash
fxcored keys show p1p2p3 -o json

fxcored keys show p1p2p3 --show-multisig
```

The first step to create a multisig transaction is to initiate it on behalf of the multisig address created above:

> Be sure to note that for multisig accounts, if you were to create any transaction, for example `--from=<multisig_account>` your `<multisig_account>` needs to be the wallet address ie `fx123l3kjltjwlfgjslfg....`. Only for those non-multisig accounts can you use the name of the account ie `--from=heimendinger`.

```bash
fxcored tx bank send fx1570v2fq3twt0f0x02vhxpuzc9jc4yl30q2qned fx12u8ekfqdd75r4apyqv2xst6qw0n3wvr2asncf5 1000000000000000000FX \
  --gas="auto" \
  --gas-adjustment=1.5 \
  --gas-prices="4000000000000FX" \
  --generate-only > unsignedTx.json
```

The file `unsignedTx.json` contains the unsigned transaction encoded in JSON. `p1` can now sign the transaction with its own private key:

```bash
fxcored tx sign \
  unsignedTx.json \
  --multisig=<multisig_address> \
  --from=p1 \
  --output-document=p1signature.json \
  --chain-id=dhobyghaut
```

Once the signature is generated, `p1` transmits both `unsignedTx.json` and `p1signature.json` to `p2` or `p3`, which in turn will generate their respective signature:

```bash
fxcored tx sign \
  unsignedTx.json \
  --multisig=<multisig_address> \
  --from=p2 \
  --output-document=p2signature.json \
  --chain-id=dhobyghaut
```

{% hint style="info" %}
for Mainnet the ChainId should be **fxcore**
{% endhint %}

`p1p2p3` is a 2-of-3 multisig key, therefore one additional signature is sufficient. Any the key holders can now generate the multisig transaction by combining the required signature files:

```bash
fxcored tx multisign \
  unsignedTx.json \
  p1p2p3 \
  p1signature.json p2signature.json > signedTx.json
```

The transaction can now be sent to the node:

```bash
fxcored tx broadcast signedTx.json
```

## Shells Completion Scripts

Completion scripts for popular UNIX shell interpreters such as `Bash` and `Zsh` can be generated through the `completion` command, which is available for both `fxcored` and `fxcored`.

If you want to generate `Bash` completion scripts run the following command:

```bash
fxcored completion > fxcored_completion
fxcored completion > fxcorecli_completion
```

If you want to generate `Zsh` completion scripts run the following command:

```bash
fxcored completion --zsh > fxcored_completion
fxcored completion --zsh > fxcorecli_completion
```

> Note: On most UNIX systems, such scripts may be loaded in `.bashrc` or `.bash_profile` to enable Bash autocompletion:

```bash
echo '. fxcored_completion' >> ~/.bashrc
echo '. fxcorecli_completion' >> ~/.bashrc
```

> Refer to the user's manual of your interpreter provided by your operating system for information on how to enable shell autocompletion.
