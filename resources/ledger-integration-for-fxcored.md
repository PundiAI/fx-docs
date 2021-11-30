# Ledger Integration for fxcored

Using a hardware wallet to store your keys greatly improves the security of your crypto assets. The Ledger device acts as an enclave of the seed and private keys, and the process of signing transaction takes place within it. No private information ever leaves the Ledger device. The following is a short tutorial on using the Cosmos Ledger app with the f(x)Core CLI.

At the core of a Ledger device there is a mnemonic seed phrase that is used to generate private keys. This phrase is generated when you initialize your Ledger. The mnemonic is compatible with Cosmos and can be used to seed new accounts.

{% hint style="info" %}
Do not lose or share your 24 words with anyone. To prevent theft or loss of funds, it is best to keep multiple copies of your mnemonic stored in safe, secure places. If someone is able to gain access to your mnemonic, they will fully control the accounts associated with them.
{% endhint %}

## Install the Cosmos Ledger application

Installing the `Cosmos` application on your ledger device is required before you can use it with our f(x)Core CLI. To do so, you need to:

### Before you start

* Install [Ledger Live](https://shop.ledger.com/pages/ledger-live) on your machine.
* Using Ledger Live, [update your Ledger Nano S with the latest firmware](https://support.ledger.com/hc/en-us/articles/360002731113-Update-device-firmware)/[Ledger Nano X](https://support.ledger.com/hc/en-us/articles/360018784134-Set-up-your-Ledger-Nano-X?docs=true).

### Install the Cosmos (ATOM) app on your Ledger device

1. Open **Ledger Live** and navigate to the **Manager** tab.
2. Connect and unlock your Ledger **device**.
3. If asked, allow the manager on your device.
4. Search for the **Cosmos (ATOM)** app in the app catalog.
5. Click the **Install** button to install the app on your Ledger device.
   * Your Ledger device displays **Processing.**
   * Ledger Live displays **Installed.**
6. More information on how to set up your Ledger device can be found [here](https://support.ledger.com/hc/en-us/articles/360013713840-Cosmos-ATOM-?docs=true).

> To see the `Cosmos` application when you search for it, you might need to activate the `Developer Mode`, located in the Experimental features tab of the Ledger Live application.

## f(x)Core CLI + Ledger Nano

**Note: You need to** [**install the Cosmos app**](ledger-integration-for-fxcored.md#install-the-cosmos-ledger-application) **on your Ledger Nano before moving on to this section**

The tool used to generate addresses and transactions on the f(x)Core network is `fxcored`. You will be using fxcored CLI commands for creating transactions and then using your Ledger to sign off before broadcasting the transaction to a specified node using the fxcored CLI.

### Install f(x)Core

> **You need to** [**install f(x)Core**](../f-x-core/installation.md) **before you proceed further**

### Add your Ledger key

* Connect and unlock your Ledger device.
* Open the Cosmos app on your Ledger.
* Create an account in fxcored from your ledger key.

> Be sure to change the `_name` parameter to be a meaningful name. The `--ledger` flag tells `fxcored` to use your Ledger to seed the account.

```bash
fxcored keys add <_name> --ledger
```

Cosmos uses [HD Wallets](https://www.ledger.com/academy/crypto/what-are-hierarchical-deterministic-hd-wallets). This means you can setup multiple accounts using the same Ledger seed. To create another account from your Ledger device, run the following, (changing the integer \<i> to some value >= 0 to choose the account for HD derivation):

```bash
fxcored keys add <secondKeyName> --ledger --account <i>
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
fxcored keys add heizenberg \
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

### Confirm your address

Run this command to display your address on your Ledger device. Use the `_name` you gave your ledger key. The `-d` flag is supported in version `1.5.0` and higher.

```bash
fxcored keys show <_name> -d
```

Confirm that the address displayed on the device matches the address displayed when you first added the key above.

### Connect to a full node

Next, you need to configure fxcored with the URL of a Cosmos full node and the appropriate `chain_id`. In this example we connect to the public load balanced full node operated by Chorus One on the `cosmoshub-2` chain. But you can point your `fxcored` to any Cosmos full node. Be sure that the `chain-id` is set to the same chain as the full node.

```bash
# configuring to a full node
fxcored config <config file name> <host>:<port>
# example: fxcored config config.toml rpc.laddr https://77.87.106.33:26657

# configuring the chain-id
fxcored config <config file name> <chain-id>
# example: fxcored config config.toml chain-id dhobyghaut
```

Test your connection with a query such as:

```bash
fxcored query staking validators
```

> **To run your own full node locally read more** [**here**](../f-x-core/setup-node/)**.**

### Sign a transaction

You are now ready to start signing and sending transactions. Send a transaction with fxcored using the `tx bank send` command.

```bash
fxcored tx bank send --help # to see all available options.
```

{% hint style="info" %}
Be sure to unlock your device with the PIN and open the Cosmos app before trying to run these commands

Use the `_name` you set for your Ledger key and f(x)Core will connect with the Cosmos Ledger app to then sign your transaction.
{% endhint %}

```bash
fxcored tx bank send <_name> <destinationAddress> <amount><denomination>
```

Assuming you added the `--keyring-backend <file>` flag earlier, an example of a transaction would look like the following:

{% hint style="info" %}
If you are not running a node, be sure to add in the `--node` flag to specify which node you would like to broadcast your transaction.
{% endhint %}

```
fxcored tx bank send robstark fx10pcel8r4jt6l6p8rff6nwz3ghq4f7z0jc8qhtg 1000000000000000000FX \
  --gas="auto" \
  --gas-adjustment=1.5 \
  --gas-prices="4000000000000FX" \
  --node tcp://47.111.20.171:26657 \
  --ledger \
  --keyring-backend file
```

You will be prompted to enter the passphrase for the `--keyring-backend` flag:

```
Enter keyring passphrase:
```

Once inputted the correct passphrase:

```
Default sign-mode 'direct' not supported by Ledger, using sign-mode 'amino-json'.
gas estimate: 79288
{"body":{"messages":[{"@type":"/cosmos.bank.v1beta1.MsgSend","from_address":"fx18pcel8r1jt6l6p8rff6nwz3ghq2f7z0jc8qhtg","to_address":"fx10pcel8r4jt6l6p8rff6nwz3ghq4f7z0jc8qhtg","amount":[{"denom":"FX","amount":"1000000000000000000"}]}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[{"denom":"FX","amount":"317152000000000000"}],"gas_limit":"79288","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]:
```

After inputting `y`, you will be prompted to review and approve the transaction on your Ledger device.`View Transaction` on your Ledger, be sure to inspect the transaction JSON displayed on the screen. You can scroll through each field and each message. You may refer [here](ledger-integration-for-fxcored.md#the-f-x-core-standard-transaction) to read more about the data fields of a standard transaction object. When prompted with `confirm transaction before signing`, Answer `Y`.

### Receive funds

To receive funds to the `fx` account on your Ledger device, retrieve the address for your Ledger account (the ones with `TYPE ledger`) with this command:

```bash
fxcored keys list

- name: john117
  type: ledger
  address: fx123123sfgsfgsfdghgfjdjdjdhdgj
  pubkey: fxpub1addwnpepqfq6g4123sfgsfgsfdghgfjdjdjdhdgjsfgsfgsfdghgfjdjdjdhdgj
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

### Further documentation

Not sure what `fxcored` can do? Simply run the command without arguments to output documentation for the commands in supports.

> The `fxcored` help commands are nested. So `$ fxcored` will output docs for the top level commands (status, config, query, and tx). You can access documentation for sub commands with further help commands.

For example, to print the `query` commands:

```bash
fxcored query --help
```

Or to print the `tx` (transaction) commands:

```bash
fxcored tx --help
```

## The f(x)Core Standard Transaction

Transactions in fxcore embed the [Standard Transaction type](https://godoc.org/github.com/cosmos/cosmos-sdk/x/auth#StdTx) from the Cosmos SDK. The Ledger device displays a serialized JSON representation of this object for you to review before signing the transaction. Here are the fields and what they mean:

* `chain-id`: The chain to which you are broadcasting the tx, such as the `fxcore` dhobyghaut or `fxcore`: mainnet.
* `account_number`: The global id of the sending account assigned when the account receives funds for the first time.
* `sequence`: The nonce for this account, incremented with each transaction.
* `fee`: JSON object describing the transaction fee, its gas amount and coin denomination
* `memo`: optional text field used in various ways to tag transactions.
* `msgs_<index>/<field>`: The array of messages included in the transaction. Double click to drill down into nested fields of the JSON.
