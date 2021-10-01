# Run a Validator on the f(x)Core Mainnet

> Information on how to join the mainnet (`genesis.json` file and seeds) is held in our `fxcore` repo. 

Before setting up your validator node, make sure you've already gone through the `Full Node Setup` guide either with [Binaries](../tutorials/full-node-with-binaries.md) or with [Docker](../tutorials/full-node-with-docker.md).

If you plan to use a KMS (key management system), you should go through these steps first.

## What is a Validator?

Validators are responsible for committing new blocks to the blockchain through voting. A validator's stake is slashed if they become unavailable or sign blocks at the same height.

Before you proceed to the next section, ensure that you have already `set up a full-node`.

## Create Your Validator

> ⚠️ We support ledger for sending transactions, we recommend using ledger as it is more secure, note that such transactions require fxcore to be [installed](../tutorials/installation.md) on both the remote vm and the host vm, which is a bit of a pain but worth doing.

* Create validator (this will be your validator's token holding account)

```bash
# For ledger
fxcored keys add <_name> --ledger --account 0
or
# Without ledger
fxcored keys add <_name>
```

For example:

```bash
fxcored keys add test --ledger --account 0

return
  name: test
  type: ledger
  address: "..." 
  pubkey: "..."
  mnemonic: ""
  threshold: 0
  pubkeys: []
  
```

* Get validator pubkey

Your `fxvalconspub` can be used to create a new validator by staking tokens (this is the account used by the node consensus). You can find your validator pubkey by running:

```bash
fxcored tendermint show-validator
```
* To check if node is running
```bash
ps -ef | grep fxcored
```

* Create-validator (this is to bind the node consensus and token holding accounts)

> ⚠️ ⚠️ ⚠️ Be sure to check that the entire node has been synchronized to the latest block height before sending the create verifier transaction, using `fxCored status` to check  `"catching_up": false`, otherwise you risk being jailed

Please ensure that your token holding account has enough `FX tokens` before creating a validator.
For `Testnet version`, you may obtain `FX tokens` via [FX Faucet](https://aabbcc-faucet.functionx.io/).
For more information on how to obtain `FX tokens` on [Testnet](../resources/testnet-fxwallet.md).

> In the `amount` field, do not use more `FX` than you have! Some `FX` is needed to `create the validator`.
> The main fields to change are `amount` and `from`

```bash
fxcored tx staking create-validator \
  --chain-id=fxcore \
  --gas="auto" \
  --gas-adjustment=1.2 \
  --gas-prices="6000000000000FX" \
  --from=<_name> \
  --amount=100000000000000000000000FX \
  --pubkey=$(fxcored tendermint show-validator) \
  --moniker="choose a moniker" \
  --commission-rate="0.01" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1000000000000000000" \
  --moniker="choose a moniker" \
  --website="https://functionx.io" \
  --details="To infinity and beyond!" 
```

Return：

```bash
{"body":{"messages":[{"@type":"/cosmos.staking.v1beta1.MsgCreateValidator","description":{"moniker":"choose a moniker","identity":"","website":"","security_contact":"","details":""},"commission":{"rate":"0.010000000000000000","max_rate":"0.200000000000000000","max_change_rate":"0.010000000000000000"},"min_self_delegation":"1000000000000000000","delegator_address":"fx1egmy0ncxzuur504qlz9z0ykfa5cqdk0ap5tgxz","validator_address":"fxvaloper1egmy0ncxzuur504qlz9z0ykfa5cqdk0af9khcz","pubkey":{"@type":"/cosmos.crypto.ed25519.PubKey","key":"JWar8+3FHVppCQWH7S4w27eMhjkyxXqUYmnbo185B3g="},"value":{"denom":"FX","amount":"500000000000000000000"}}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[{"denom":"FX","amount":"1020264000000000000"}],"gas_limit":"170044","payer":"","granter":""}},"signatures":[]}

{"height":"729953","txhash":"8FB0EDE90AE37D6603D7FD3278018A7897E243F2DEF69F0592FF71BC58B40AE2","codespace":"","code":0,"data":"0A120A106372656174655F76616C696461746F72","raw_log":"[{\"events\":[{\"type\":\"create_validator\",\"attributes\":[{\"key\":\"validator\",\"value\":\"fxvaloper1egmy0ncxzuur504qlz9z0ykfa5cqdk0af9khcz\"},{\"key\":\"amount\",\"value\":\"500000000000000000000\"}]},{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"create_validator\"},{\"key\":\"module\",\"value\":\"staking\"},{\"key\":\"sender\",\"value\":\"fx1egmy0ncxzuur504qlz9z0ykfa5cqdk0ap5tgxz\"}]}]}]","logs":[{"msg_index":0,"log":"","events":[{"type":"create_validator","attributes":[{"key":"validator","value":"fxvaloper1egmy0ncxzuur504qlz9z0ykfa5cqdk0af9khcz"},{"key":"amount","value":"500000000000000000000"}]},{"type":"message","attributes":[{"key":"action","value":"create_validator"},{"key":"module","value":"staking"},{"key":"sender","value":"fx1egmy0ncxzuur504qlz9z0ykfa5cqdk0ap5tgxz"}]}]}],"info":"","gas_wanted":"170044","gas_used":"155067","tx":null,"timestamp":""}
```

> When specifying commission parameters, the `commission-max-change-rate` is used to measure % _point_ change over the `commission-rate`. E.g. 1% to 2% is a 100% rate increase, but only 1 percentage point.

> `Min-self-delegation` is a stritly positive integer that represents the minimum amount of self-delegated voting power your validator must always have. A `min-self-delegation` of `100000000000000000000` means your validator will never have a self-delegation lower than `100` $FX

* Check Validator

After "send create-validator" you will get an object data from the terminal with code = 0 indicating successful creation.

```bash
# delegator_address from Send create-validator return result
fxcored query staking validator <validator_address> 
```
You can confirm that you are in the validator set by using a third party explorer for [Testnet](https://aabbcc-explorer.functionx.io/validator)/[Mainnet](https://explorer.functionx.io/).

## Edit Validator Description

You can edit your validator's public description. This info is to identify your validator, and will be relied on by delegators to decide which validators to stake to. Make sure to provide input for every flag below. If a flag is not included in the command the field will default to empty (`--moniker` defaults to the machine name) if the field has never been set or remain the same if it has been set in the past.

The <key_name> specifies which validator you are editing. If you choose to not include certain flags, remember that the --from flag must be included to identify the validator to update.

The `--identity` can be used as to verify identity with systems like Keybase or UPort. When using with Keybase `--identity` should be populated with a 16-digit string that is generated with a [keybase.io](https://keybase.io) account. It's a cryptographically secure method of verifying your identity across multiple online networks. The Keybase API allows us to retrieve your Keybase avatar. This is how you can add a logo to your validator profile.

```bash
fxcored tx staking edit-validator
  --moniker="choose a moniker" \
  --website="https://functionx.io" \
  --identity=6A0D65E29A4CBC8E \
  --details="To infinity and beyond!" \
  --commission-rate="0.10"
  --chain-id=fxcore \
  --gas="auto" \
  --gas-adjustment=1.2 \
  --gas-prices="6000000000000FX" \
  --from=<key_name> \
```

__Note__: The `commission-rate` value must adhere to the following invariants:

- Must be between 0 and the validator's `commission-max-rate`
- Must not exceed the validator's `commission-max-change-rate` which is maximum
  % point change rate **per day**. In other words, a validator can only change
  its commission once per day and within `commission-max-change-rate` bounds.

## View Validator Description

View the validator's information with this command:

```bash
fxcored query staking validator <validator_address>
```

## Track Validator Signing Information

In order to keep track of a validator's signatures in the past you can do so by using the `signing-info` command:

```bash
fxcored query slashing signing-info "$(fxcored tendermint show-validator)"
```

## Unjail Validator

When a validator is "jailed" for downtime, you must submit an `Unjail` transaction from the operator account in order to be able to get block proposer rewards again (depends on the zone fee distribution).

```bash
fxcored tx slashing unjail --from=<key_name>
```

## Confirm Your Validator is Running

Your validator is active if the following command returns anything:

```bash
fxcored query tendermint-validator-set | grep "$(fxcored tendermint show-address)"
```

You should now see your validator in one of the f(x)Core explorers. You are looking for the `bech32` encoded `address` in the `~/.fxcore/config/priv_validator.json` file.


> To be in the validator set, you need to have more total voting power than the 100th validator.


## Halting Your Validator

When attempting to perform routine maintenance or planning for an upcoming coordinated
upgrade, it can be useful to have your validator systematically and gracefully halt.
You can achieve this by either setting the `halt-height` to the height at which
you want your node to shutdown or by passing the `--halt-height` flag to `fxcored`.
The node will shutdown with a zero exit code at that given height after committing
the block.

## Common Problems

### Problem #1: My validator has `voting_power: 0`

Your validator has become jailed. Validators get jailed, i.e. get removed from the active validator set, if they do not vote on `500` of the last `10000` blocks, or if they double sign. 

If you got jailed for downtime, you can get your voting power back to your validator. First, if `fxcored` is not running, start it up again:

```bash
fxcored start
```

Wait for your full node to catch up to the latest block. Then, you can [unjail your validator](#unjail-validator)

Lastly, check your validator again to see if your voting power is back.

```bash
fxcored status
```

You may notice that your voting power is less than it used to be. That's because you got slashed for downtime!

### Problem #2: My `fxcored` crashes because of `too many open files`

The default number of files Linux can open (per-process) is `1024`. `fxcored` is known to open more than `1024` files. This causes the process to crash. A quick fix is to run `ulimit -n 4096` (increase the number of open files allowed) and then restart the process with `fxcored start`. If you are using `systemd` or another process manager to launch `fxcored` this may require some configuration at that level. A sample `systemd` file to fix this issue is below:

```text
# /etc/systemd/system/fxcored.service
[Unit]
Description=f(x)Core Node
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu
ExecStart=/home/ubuntu/go/bin/fxcored start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
```
