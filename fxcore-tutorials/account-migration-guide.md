# Account Migration Guide (CLI)

### f(x)Core Account Migration Guide --support EVM compatibility

This upgrade introduces a new address which will enable `ethereum` compatibility.

Things to note❗:

a. This guide can be done on f(x)Core Mainnet & Testnet.

b. Ensure the [new Evm upgrade](../upgrade-instructions/manual/README.md) is done.

c. Ensure you have f(x)Core [command line interface](../fxcore/installation) installed.

d. Your address should not be bonded to any validator (Address should not have a prior successful `create-validator` command).

**Accounts in f(x)Core**

`Accounts in f(x)Core` can be represented in both Bech32 and hex format for Ethereum's Web3 tooling compatibility.

* The Bech32 format is the default format for Cosmos-SDK queries and transactions through CLI and REST clients.
* Address (Bech32) with the prefix `fx` eg: `fx1xzyws0l8p8alt6v7tztvqlph8r22lhn4femgr7`
* Address (EIP55 Hex) with the prefix `0x` eg: `0x3088e83FE709fBf5e99e5896C07c3738d4aFDE75`&#x20;
* The hex format on the other hand, is the Ethereum `common.Address` representation of a Cosmos `sdk.AccAddress`
* Compressed **Public Key** (Bech32): (used to **encrypt** data) `fxpub17weu6qepqgca6ed53q7frh8ftcr6c0kucfhm0yuat4sxx4hc3u2y7pydcwwyc65xl7f`

### Upgrade Steps

#### **1. Prepare the account with the fx address prefix before the migration**

**Prerequisites:**

* This account must have on-chain assets
* This account must have made an on-chain transaction before if not it's data would not be registered on-chain

Prepare a wallet/key derived with **secp256k1** curve, which Cosmos hub uses to identify for accounts:

{% hint style="info" %}
if it is a mnemonics import, by specifying --recover (import) and an --index (import) the specific index account of that mnemonic
{% endhint %}

{% tabs %}
{% tab title="create fx prefix address" %}
```shell
fxcored keys add <fx_key_name> --algo secp256k1 --coin-type 118 --index <index_number>
```
{% endtab %}

{% tab title="recover fx prefix address" %}
```shell
fxcored keys add <fx_key_name> --algo secp256k1 --coin-type 118 --index <index_number> --recover
```
{% endtab %}
{% endtabs %}

example:

```bash
fxcored keys add fx-0 --algo secp256k1 --coin-type 118 --index 0
```

{% hint style="info" %}
You will need to store your mnemonic phrase in a safe location. You may use the [faucet link](https://testnet-faucet.functionx.io/) to get some test funds in your wallet.

You may skip the 1st step, if you already have a wallet/key that is derived using the `"secp256k1"` algorithm. Use `fxcored keys list` to check for all keys that are present in your directory.
{% endhint %}

#### **2. Prepare the 0x prefix address account (Ethereum format address)**

> By default, the `fxcored keys add` command is defaulted to `--algo=eth_secp256k1 --coin-type=60` without specifying the flag, specifying it will have the same result.

**Prerequisites**

* This account must not have sent an on-chain transaction

Prepare a new wallet/key derived  using the `eth_secp256k1` curve after migration:

```shell
fxcored keys add <0x_key_name> --algo eth_secp256k1 --coin-type 60 --index <index_number>
```

example:

```bash
fxcored keys add 0x-0 --algo eth_secp256k1 --coin-type 60 --index 0
```

#### 3. **Do ensure fx prefix address accounts and 0x prefix address accounts must be in the same keyring-backend file**

```shell
fxcored keys list
```

{% hint style="info" %}
From the above returned results from the query, the \<fx-0> address account and the < 0x-0> address account must be in the same directory. Also you may double check that \<fx-0> and <0x-0>'s algo should be secp256k1 and eth\_secp256k1 respectively.
{% endhint %}

#### **4. Account Check Before Migration**

```bash
fxcored query migrate account $(fxcored keys show fx-0 -a)
```

The result should show the account balance, delegation, de-delegate, re-delegate, and votes, if any.

```shell
fxcored query migrate account $(fxcored keys show 0x-0 -e)
```

The result fields should display null as <0x-0> is a brand new wallet.

#### 5. Send the migration transaction on fxcored

```bash
fxcored tx migrate account $(fxcored keys show <0x_key_name> -e) \
--from <fx-0_keyName> --gas-adjustment 1.5 --gas="auto" --gas-prices=4000000000000FX \
--chain-id=<chain-id>
```

**Migration of address**

The following command will move **everything** in the previous old `<fx-0>` address, such as token assets, delegations, delegating rewards, and proposals, if any.

#### 6. Account check after migration

```bash
fxcored query migrate account $(fxcored keys show fx-0 -a)
```

The results returned from the above query should show the account balance of \<fx-0> address (FX, PUNDIX, PURSE...), delegate, de-delegate, re-delegate, and vote as either null or zero.

```bash
fxcored query migrate account $(fxcored keys show 0x-0 -e)
```

The results returned from the above query should show account balance of <0x-0> address (FX, PUNDIX, PURSE...), delegate, de-delegate, re-delegate, and votes.

```
fxcored keys export <0x-0_keyName> --unarmored-hex --unsafe
```

Now you can **export your private key** from the f(x)Core **terminal** using the following command. Again, make sure to replace <0x-0\_keyName> with the name of the key that you want to export and use the correct `keyring-backend`:

## Recover your FX account in **MetaMask**

### **Connect your MetaMask wallet**

The MetaMask browser extension is a wallet for accessing Ethereum-enabled applications and managing user identities.

### **Adding a New Network**

Open the MetaMask extension on your browser, you may have to log in to your MetaMask account if you are not already. Then click the top right circle and go to `Settings` > `Networks` > `Add Network` and fill the form as shown below.

Here is the **list of fields** that you can use to paste on Metamask:

* **Network Name:** `DhobyGhaut Testnet`
* **New RPC URL:** `https://testnet-fx-json-web3.functionx.io:8545`
* **Chain ID:** `90001`
* **Currency Symbol (optional):** `FX`

> For more details, you may refer to below page:

{% content-ref url="../deploying-on-fxcore-evm/metamask/add-fxcore-network.md" %}
[add-fxcore-network.md](../deploying-on-fxcore-evm/metamask/add-fxcore-network.md)
{% endcontent-ref %}

### **Import Account to Metamask**

Once you have added `DhobyGhaut Testnet` to the Metamask `Networks`, you can automatically import your accounts by:

#### **Manual Import**

Close the `Settings`, go to `My Accounts` (top right circle) and select `Import Account`. You should see an image like the following one:

![](<../.gitbook/assets/image (9).png>)

In Metamask and select the `Private Key` option. Then **paste the private** key(**hexadecimal**) exported from the command in step 6, to import your new EVM-compatible account in MetaMask.

Your account balance will show up, as such. If it takes some time to load the balance of the account, change the network to `Main Ethereum Network` and then switch back to `DhobyGhaut Testnet`.Once you have imported your wallet in MetaMask, you can transfer your assets to the EVM-compatible wallet address and spend within Dapps in the f(x)Core ecosystem.



