# Binaries - Upgrading Your Node

### f(x)Core Network Upgrades

{% hint style="warning" %}
<mark style="color:yellow;">**WARNING**</mark>

There are 2 types of network upgrades with similar steps but it is IMPORTANT to differentiate and identify which type of upgrade is required and which to perform before the upgrade height is reached. Hard fork requires validators to upgrade their nodes before the upgrade height and software upgrade requires validators to do so after the upgrade height.



#### **Hard fork upgrade:**

The code after the upgrade is backward compatible, so the node _**can (and needs to) be updated before the upgrade height**_, and when the upgrade height is reached, the node will automatically switch to the new logic.



**Software upgrade:**

When the upgrade proposal is passed, _**we need to wait for the block height to reach the upgrade height set in the proposal. We cannot use the new program to update the node in advance**_, because the code after the upgrade is backward incompatible. When the block height reaches the upgrade height, the node will automatically stop producing blocks and print the log: "ERR UPGRADE" upgrade proposal plan name "NEEDED at height: upgrade proposal set height...", and then we can use the latest program to update the node
{% endhint %}

> For more information on past upgrades and instructions, refer to [**Upgrade Versions**](../upgrade-versions/).
>
> You may refer to this [**Countdown Timer**](https://functionx.github.io/fx-upgrade/index.html) which will countdown the time till the upgrade height.

### Upgrade steps

1. Ensure you have stopped the node❗

{% tabs %}
{% tab title="With Daemon" %}
```
sudo systemctl stop fxcored
```
{% endtab %}

{% tab title="With PID" %}
```
ps -ef | grep fxcored
kill -9 <PID>
```
{% endtab %}
{% endtabs %}

2\. Pulling the latest fx-core code base (ensure that you are in the fx-core folder):

```
git pull
```

3\. Checkout the branch of the upgrade version:

```shell
git checkout <upgradeable version branch>
```

for example:

```
git checkout tags/v2.2.1 -b release/v2.2.x
```

Check log whether is the latest commit

```
git log
```

> <mark style="color:orange;">**commit 5eef07630e89ad0bd786fb08fa0fb937e5c84d67 (**</mark><mark style="color:blue;">**HEAD -**</mark>**> **<mark style="color:green;">**release/v2.2.x**</mark><mark style="color:yellow;">**,**</mark>**  **<mark style="color:yellow;">**tag: v2.2.1)**</mark>

4\. Update fxcored (ensure that you are in the fx-core folder):

```
make go.sum
make install
```

5\. Cross reference the latest commit hash to the commit in our [official github page](https://github.com/FunctionX/fx-core):

```
fxcored version
```

6\. Download genesis (copy and run each line, line by line)

{% tabs %}
{% tab title="Mainnet" %}
```
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.2.x/public/mainnet/genesis.json -O ~/.fxcore/config/genesis.json
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.2.x/public/mainnet/config.toml -O ~/.fxcore/config/config.toml
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.2.x/public/mainnet/app.toml -O ~/.fxcore/config/app.toml
```
{% endtab %}

{% tab title="Testnet" %}
```
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.2.x/public/testnet/genesis.json -O ~/.fxcore/config/genesis.json
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.2.x/public/testnet/config.toml -O ~/.fxcore/config/config.toml
wget https://raw.githubusercontent.com/FunctionX/fx-core/release/v2.2.x/public/testnet/app.toml -O ~/.fxcore/config/app.toml
```
{% endtab %}
{% endtabs %}

7\. Restart the node:

```
sudo systemctl restart fxcored
```

8\. Check whether the node is participating in consensus:

```
cat $HOME/.fxcore/data/priv_validator_state.json
```

It should return something similar to the following:

{% hint style="info" %}
You can cross reference the block "height" field with that of the [FunctionX Explorer](https://dhobyghaut-explorer.functionx.io/fxcore/blocks)
{% endhint %}

```
{
  "height": "347450",
  "round": 0,
  "step": 3,
  "signature": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  "signbytes": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
}
```

{% hint style="success" %}
:star::star::star:Additional features of client.toml
{% endhint %}

> Users can specify the configuration of certain commands in the configuration file `client.toml`
>
> Configure priority --flag> client.toml(default)
>
> A point to note is that client.toml configuration are for fxcored CLI, while app.toml and config.toml are configurations for the node.

| key               | default                 | Optional value         | description                                                                                                                 |
| ----------------- | ----------------------- | ---------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `chain-id`        |                         | `fxcore`               | Chain ID-used when signing transactions                                                                                     |
| `keyring-backend` | `os`                    | `os`/`file`/`test`     | Keys storage method os: stored in the system password, file: file, specified password encryption, test: file, no encryption |
| `output`          | `text`                  | `text`/`json`          | Output format when querying                                                                                                 |
| `node`            | `tcp://localhost:26657` |                        | Node address to be called                                                                                                   |
| `broadcast-mode`  | `sync`                  | `sync`/`async`/`block` | Broadcast transaction mode                                                                                                  |

Modify client.toml command `fxcored config $key $value` (example):

```
fxcored config chain-id fxcore

fxcored config keyring-backend test

fxcored config output json

fxcored config node "tcp://127.0.0.1:26657"

fxcored config broadcast-mode block
```
