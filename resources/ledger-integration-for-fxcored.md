# Ledger Integration for fxcored

Using a hardware wallet to store your keys greatly improves the security of your crypto assets. The Ledger device acts as an enclave of the seed and private keys, and the process of signing transaction takes place within it. No private information ever leaves the Ledger device. The following is a short tutorial on using the Cosmos Ledger app with the f(x)Core CLI.

At the core of a Ledger device there is a mnemonic seed phrase that is used to generate private keys. This phrase is generated when you initialize you Ledger. The mnemonic is compatible with Cosmos and can be used to seed new accounts.

{% hint style="info" %}
Do not lose or share your 24 words with anyone. To prevent theft or loss of funds, it is best to keep multiple copies of your mnemonic stored in safe, secure places. If someone is able to gain access to your mnemonic, they will fully control the accounts associated with them.
{% endhint %}

## Install the Cosmos Ledger application

Installing the `Cosmos` application on your ledger device is required before you can use it with our f(x)Core CLI. To do so, you need to:

1. Install [Ledger Live](https://shop.ledger.com/pages/ledger-live) on your machine.
2. Using Ledger Live, [update your Ledger Nano S with the latest firmware](https://support.ledger.com/hc/en-us/articles/360002731113-Update-device-firmware)/[Ledger Nano X](https://support.ledger.com/hc/en-us/articles/360018784134-Set-up-your-Ledger-Nano-X?docs=true).
3. On the Ledger Live application, navigate to the `Manager` menu .
4. Connect your Ledger Nano device and allow Ledger Manager from it.
5. On the Ledger Live application, Search for `Cosmos`.
6. Install the Cosmos application by clicking on `Install`.

> To see the `Cosmos` application when you search for it, you might need to activate the `Developer Mode`, located in the Experimental features tab of the Ledger Live application.

## f(x)Core CLI + Ledger Nano

**Note: You need to **[**install the Cosmos app**](ledger-integration-for-fxcored.md#install-the-cosmos-ledger-application)** on your Ledger Nano before moving on to this section**

The tool used to generate addresses and transactions on the f(x)Core network is `fxcored`. Here is how to get started.

### Install f(x)Core

> **You need to **[**install f(x)Core**](ledger-integration-for-fxcored.md#install-f-x-core)** before you go further**

### Add your Ledger key

* Connect and unlock your Ledger device.
* Open the Cosmos app on your Ledger.
* Create an account in fxcored from your ledger key.

> Be sure to change the `_name` parameter to be a meaningful name. The `--ledger` flag tells `fxcored` to use your Ledger to seed the account.

```bash
fxcored keys add <_name> --ledger
```

Cosmos uses [HD Wallets](broken-reference). This means you can setup many accounts using the same Ledger seed. To create another account from your Ledger device, run (change the integer \<i> to some value >= 0 to choose the account for HD derivation):

```bash
fxcored keys add <secondKeyName> --ledger --account <i>
```

### Confirm your address

Run this command to display your address on the device. Use the `_name` you gave your ledger key. The `-d` flag is supported in version `1.5.0` and higher.

```bash
fxcored keys show <_name> -d
```

Confirm that the address displayed on the device matches the address displayed when you first added the key above.

### Connect to a full node

Next, you need to configure fxcored with the URL of a Cosmos full node and the appropriate `chain_id`. In this example we connect to the public load balanced full node operated by Chorus One on the `cosmoshub-2` chain. But you can point your `fxcored` to any Cosmos full node. Be sure that the `chain-id` is set to the same chain as the full node.

```bash
fxcored config node https://cosmos.chorus.one:26657
fxcored config chain_id cosmoshub-2
```

Test your connection with a query such as:

```bash
fxcored query staking validators
```

::: tip To run your own full node locally [read more here.](https://cosmos.network/docs/cosmos-hub/join-mainnet.html#setting-up-a-new-node). :::

### Sign a transaction

You are now ready to start signing and sending transactions. Send a transaction with fxcored using the `tx send` command.

```bash
fxcored tx send --help # to see all available options.
```

::: tip Be sure to unlock your device with the PIN and open the Cosmos app before trying to run these commands :::

Use the `keyName` you set for your Ledger key and f(x)Core will connect with the Cosmos Ledger app to then sign your transaction.

```bash
fxcored tx send <keyName> <destinationAddress> <amount><denomination>
```

When prompted with `confirm transaction before signing`, Answer `Y`.

Next you will be prompted to review and approve the transaction on your Ledger device. Be sure to inspect the transaction JSON displayed on the screen. You can scroll through each field and each message. Scroll down to read more about the data fields of a standard transaction object.

Now, you are all set to start [sending transactions on the network](broken-reference).

### Receive funds

To receive funds to the Cosmos account on your Ledger device, retrieve the address for your Ledger account (the ones with `TYPE ledger`) with this command:

```bash
fxcored keys list

➜ NAME: TYPE: ADDRESS:     PUBKEY:
<keyName> ledger cosmos1... cosmospub1...
```

### Further documentation

Not sure what `fxcored` can do? Simply run the command without arguments to output documentation for the commands in supports.

::: tip The `fxcored` help commands are nested. So `$ fxcored` will output docs for the top level commands (status, config, query, and tx). You can access documentation for sub commands with further help commands.

For example, to print the `query` commands:

```bash
fxcored query --help
```

Or to print the `tx` (transaction) commands:

```bash
fxcored tx --help
```

:::

## The f(x)Core Standard Transaction

Transactions in Cosmos embed the [Standard Transaction type](https://godoc.org/github.com/cosmos/cosmos-sdk/x/auth#StdTx) from the Cosmos SDK. The Ledger device displays a serialized JSON representation of this object for you to review before signing the transaction. Here are the fields and what they mean:

* `chain-id`: The chain to which you are broadcasting the tx, such as the `fxcore` testnet or `fxcore`: mainnet.
* `account_number`: The global id of the sending account assigned when the account receives funds for the first time.
* `sequence`: The nonce for this account, incremented with each transaction.
* `fee`: JSON object describing the transaction fee, its gas amount and coin denomination
* `memo`: optional text field used in various ways to tag transactions.
* `msgs_<index>/<field>`: The array of messages included in the transaction. Double click to drill down into nested fields of the JSON.
