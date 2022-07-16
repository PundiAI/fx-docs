# Upgrading Your Node

### f(x)Core hard fork upgrade--support EVM compatibility

This upgrade introduces a new module `evm` which will enable `ethereum` compatibility.

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
git checkout release/v2.1.x
```

4\. Update fxcored (ensure that you are in the fx-core folder):

```
make go.sum
make install
```

5\. Cross reference the latest commit hash to the commit in our [official github page](https://github.com/FunctionX/fx-core):

```
fxcored version
```

6\. Running the following command will add EVM configurations to the **`app.toml`** and **`config.toml` ** files:

```
fxcored config update
```

Check evm configuration is added successfully:

```
fxcored config app.toml
```

Return (you should see an EVM configuration):

```
...
evm:
  max-tx-gas-wanted: 500000
  tracer: ""
...
json-rpc:
  address: 0.0.0.0:8545
  api:
  - eth
  - net
  - web3
  block-range-cap: 10000
  enable: true
  evm-timeout: 5e+09
  feehistory-cap: 100
  filter-cap: 200
  gas-cap: 2.5e+07
  http-idle-timeout: 1.2e+11
  http-timeout: 3e+10
  logs-cap: 10000
  txfee-cap: 1
  ws-address: 0.0.0.0:8546
...
```

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
