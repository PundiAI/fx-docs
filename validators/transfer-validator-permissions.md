# Transfer Validator Permissions

## Replace the consensus public key

If the server is hacked or the consensus public key is leaked, the validator's consensus public key can be replaced by updating the consensus public key transaction.

{% hint style="warning" %}
Before sending the replacement consensus public key transaction, it is best to start the node with the new public key first, so that after the replacement, the validator node will immediately participate in the block generation, and there will be no missing blocks
{% endhint %}

```
#command
fxcored tx staking edit-consensus-pubkey <val> <new-consensus-pubkey> --from 
addr/granter

#example
fxcored tx staking edit-consensus-pubkey fxvaloper123...
'{"@type":"/cosmos.crypto.ed25519.PubKey","key":"...."}' --from=fx456...
```

The `val` argument should be the validator’s address, for example: `fxvaloper1a73plz6w7fc8ydlwxddanc7a239kk45jnl9xwj`. In order to get the argument for `new-consensus-pubkey`, set up a new node and run the following:

```
fxcored tendermint show-validator --home /root/.fxcore-node-new
#/root/.fxcore-node-new is the path of the new node
```

Addr/granter: addr is the address controlled by the validator, such as `fx1a73plz6w7fc8ydlwxddanc7a239kk45jmwcesj`; granter is the address authorized by the validator, which is the address after transferring permissions using the transaction 👇 below, but **It is strongly recommended to update the consensus public key first, and then transfer address permissions**, because after transferring permissions, the old validator private key cannot control the validator

## Transferring Validator Permissions

### Transfer permissions

If the validator's private key is leaked, the current validator's authority can be transferred to another address by transferring permission transactions. At this time, the validator's private key will not be able to send any transactions on the chain, and the authorized address can send any transactions for the validator's address

{% hint style="warning" %}
After transferring permissions, any transaction needs to be authorized before the transaction can be sent! If the private key is not leaked, it is not recommended to transfer permissions

After transferring permissions, the old validator address cannot send transactions , and the new permissions must be used to send transactions on behalf of the old validator

The transfer transaction can be executed multiple times, for example, by validator val-1, transferred to addr-1, then the next transfer transaction sent is sent by addr-1, transferred to addr-2

Do not transfer permissions back to the validator addr.
{% endhint %}

```
#------------Transferring Permissions to New Address------------

#command
fxcored tx staking grant-privilege <val> <new-addr> --from addr

#example
fxcored tx staking grant-privilege fxvaloper1vx... fx1g7... --from fx1a7...
```

* val: Validator address, for example `fxvaloper1a73plz6w7fc8ydlwxddanc7a239kk45jnl9xwj`
* new-addr: new permissions holder, such as `fx1a73plz6w7fc8ydlwxddanc7a239kk45jmwcesj` ,
* addr: Old permissions holder, for example `fx1a73plz6w7fc8ydlwxddanc7a239kk45jmwcesj`

{% hint style="warning" %}
When transferring permissions, new-addr and addr signatures are required, as visa permissions and signature transactions are required
{% endhint %}

### Sending Transactions After Transferring Permissions <a href="#76b8" id="76b8"></a>

#### Authorization

```Bash
#------------1. Constructing an Authorized MsgSend Transaction------------
fxcored tx authz grant <new-key> generic --msg-type=/cosmos.bank.v1beta1.MsgSend 
--from <old-key> --chain-id dhobyghaut --generate-only > grant_bank_send.json

#--------------2. Execute Authorize MsgSend Transaction--------------
fxcored tx authz exec grant_bank_send.json --from <new-key> --gas="auto" 
--gas-adjustment=1.5 --gas-prices="4000000000000FX" --keyring-backend test 
--chain-id dhobyghaut
```

#### Transfer

```Bash
#------------1. Construct the MsgSend transaction------------
fxcored tx bank send <val-addr> <new-addr> 1FX --chain-id dhobyghaut 
--generate-only > bank_send.json

#------------2. Execute the MsgSend transaction------------
fxcored tx authz exec bank_send.json --from <new-addr>--gas="auto" 
--gas-adjustment=1.5 --gas-prices="4000000000000FX" --keyring-backend test 
--chain-id dhobyghaut
```
