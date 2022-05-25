# Account Migration Guide

### f(x)Core Account Migration Guide --support EVM compatibility

This upgrade introduces a new address which will enable `ethereum` compatibility.

Things to note❗:

a. This guide can be done only on f(x)Core Testnet.

c. Ensure the [new Evm upgrade](https://functionx.gitbook.io/home/f-x-core/setup-node/evm-upgrade-tutorial) is done.

d. Ensure you have f(x)Core [command line interface](https://functionx.gitbook.io/home/f-x-core/installation) installed.

e. Your address should not be bonded to any validator (Address should not have a prior successful `create-validator` command).

**Accounts in f(x)Core**

`Accounts in f(x)Core` can be represented in both Bech32 and hex format for Ethereum's Web3 tooling compatibility.

The Bech32 format is the default format for Cosmos-SDK queries and transactions through CLI and REST clients.

* Address (Bech32): `fx1xzyws0l8p8alt6v7tztvqlph8r22lhn4femgr7`
* Address (EIP55 Hex): `0x3088e83FE709fBf5e99e5896C07c3738d4aFDE75` The hex format on the other hand, is the Ethereum `common.Address` representation of a Cosmos `sdk.AccAddress`
* Compressed **Public Key** (Bech32): (used to **encrypt** data) `fxpub17weu6qepqgca6ed53q7frh8ftcr6c0kucfhm0yuat4sxx4hc3u2y7pydcwwyc65xl7f`

### Upgrade Steps

#### **1. Prepare the account with the fx address prefix before the migration**

Prepare a wallet/key derived with **secp256k1** curve, which Cosmos hub uses to identify for accounts.

```bash
fxcored keys add fx-0 --algo secp256k1 --coin-type 118 --index 0
```

You will need to store your mnemonic phrase in a safe location. You may use the [faucet link](https://testnet-faucet.functionx.io/) to get some test funds in your wallet. if necessary, you may add the `--recover` flag to import a key. This account should have assets on the chain and guarantees sufficient transaction fees.

You may skip the 1st step, if you already have a wallet/key that is derived using the `"secp256k1"` algorithm. Use `fxcored keys list` to check for all keys that are present in your directory.

#### **2. Prepare the 0x prefix address account**

Prepare a new wallet/key derived  using the `eth_secp256k1` curve after migration

```bash
fxcored keys add 0x-0 --algo eth_secp256k1 --coin-type 60 --index 0
```

```bash
fxcored keys list
```

From the above returned results from the query, the \<fx-0> address account and the < 0x-0> address account must be in the same directory. Also you may double check that \<fx-0> and    <0x-0> 's algo should be secp256k1 and eth\_secp256k1 respectively.

#### **3. Account Check Before Migration**

```bash
fxcored query migrate account $(fxcored keys show fx-0 -a)
```

The result should show the account balance, delegation, de-delegate, re-delegate, and votes, if any.

```bash
fxcored query migrate account $(fxcored keys show 0x-0 -a)
```

The result should show as null as <0x-0> is a brand new wallet.

#### 4. **Initialise the \<fx-0> Account by performing a transaction.**

The step can be skipped if \<fx-0> is not a newly created wallet/address, which means there has been previous transactions involving it.

```bash
fxcored tx bank send <fx-0 address> \
 <local-wallet-1-address> 100000000000000FX \
--gas="auto" --gas-adjustment=1.5 --gas-prices=4000000000000FX

Note: local-wallet-1 can be any other wallet imported in the user's 
local directory.
There will be some issues, if the first transaction was to do a migration, 
as <fx-0> is a brand new wallet address, which no transaction has been made. 
```

#### 5. Send the migration transaction

```bash
fxcored tx migrate account $(fxcored keys show 0x-0 -a) \
--from <fx-0_keyName> --gas-adjustment 1.5 --gas="auto" --gas-adjustment=1.5 \
 --gas-prices=4000000000000FX
```

**Migration of address**

The following command will move **everything** in the previous old `<fx-0>` address, such as token assets, delegations, delegating rewards, and proposals, if any.

#### 6. Account check after migration

```bash
fxcored query migrate account fx-0
```

The results returned from the above query should show the account balance of \<fx-0> address (FX, PUNDIX, PURSE...), delegate, de-delegate, re-delegate, and vote as either null or zero.

```bash
 fxcored query migrate account 0x-0
```

The results returned from the above query should show account balance of <0x-0> address (FX, PUNDIX, PURSE...), delegate, de-delegate, re-delegate, and votes.

```
fxcored keys export <0x-0_keyName> --unarmored-hex --unsafe
```

Now you can **export your private key** from the f(x)Core **terminal** using the following command. Again, make sure to replace <0x-0\_keyName> with the name of the key that you want to export and use the correct `keyring-backend`:

## Recover your FX account in **MetaMask**

### **Connect your MetaMask wallet**

The MetaMask  browser extension is a wallet for accessing Ethereum-enabled applications and managing user identities.

### **Adding a New Network**

Open the MetaMask extension on your browser, you may have to log in to your MetaMask account if you are not already. Then click the top right circle and go to `Settings` > `Networks` > `Add Network` and fill the form as shown below.

Here is the **list of fields** that you can use to paste on Metamask:

* **Network Name:** `DhobyGhaut Testnet`
* **New RPC URL:** `https://testnet-fx-json-web3.functionx.io:8545`
* **Chain ID:** `90001`
* **Currency Symbol (optional):** `FX`

### **Import Account to Metamask**

Once you have added `DhobyGhaut Testnet` to the Metamask `Networks`, you can automatically import your accounts by:

#### **Manual Import**

Close the `Settings`, go to `My Accounts` (top right circle) and select `Import Account`. You should see an image like the following one:

![](<../../.gitbook/assets/image (9).png>)

In Metamask and select the `Private Key` option. Then **paste the private** key(**hexadecimal**) exported from the command in step 6, to import your new EVM-compatible account in MetaMask.

Your account balance will show up, as such. If it takes some time to load the balance of the account, change the network to `Main Ethereum Network` and then switch back to `DhobyGhaut Testnet`.Once you have imported your wallet in MetaMask, you can transfer your assets to the EVM-compatible wallet address and spend within Dapps in the f(x)Core ecosystem.



