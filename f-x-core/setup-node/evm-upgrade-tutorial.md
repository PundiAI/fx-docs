# EVM Upgrade Tutorial

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

&#x20;2\. Pulling the latest fx-core code base (ensure that you are in the fx-core folder):

```
git pull
```

&#x20;3\. Checkout the `evm` branch:

{% tabs %}
{% tab title="Mainnet" %}
No available yet
{% endtab %}

{% tab title="Testnet" %}
```
git checkout evm
```
{% endtab %}
{% endtabs %}

&#x20;4\. Update fxcored (ensure that you are in the fx-core folder):

```
make go.sum
```

{% tabs %}
{% tab title="Mainnet" %}
```
make install
```
{% endtab %}

{% tab title="Testnet" %}
```
make install-testnet
```
{% endtab %}
{% endtabs %}

&#x20;5\. Check fxcored environment & version

```
fxcored network
```

Return (you should now see a field that says "EvmSupportBlock"):

```
ChainId: dhobyghaut
CrossChainSupportBscBlock: "1"
CrossChainSupportPolygonAndTronBlock: "1"
EIP155ChainID: "90001"
EvmV0SupportBlock: "408000"
EvmV1SupportBlock: "9223372036854775807"
GravityPruneValsetAndAttestationBlock: "1"
GravityValsetSlashBlock: "1"
Network: testnet
```

Cross reference the latest commit hash to the commit in our [official github page](https://github.com/FunctionX/fx-core):

```
fxcored version
```

&#x20;5\. Add EVM configuration (preferably adding it after the last line of the `app.toml` file) into the **`app.toml`** file there are multiple ways to do this, a few suggestions include opening the file in a [vi editor](https://www.cs.colostate.edu/helpdocs/vi.html) or editing it by remoting into the terminal using [Visual Studio Code](https://code.visualstudio.com/docs/remote/ssh):

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
  - txpool
  - personal
  - net
  - debug
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

&#x20;7\. Features of client.toml

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

&#x20;8\. Restart the node:

```
sudo systemctl restart fxcored
```

&#x20;9\. Check whether the node is participating in consensus:

```
cat $HOME/.fxcore/data/priv_validator_state.json
```

it should return something similar to the following:

```
{
  "height": "347450",
  "round": 0,
  "step": 3,
  "signature": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  "signbytes": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
}
```

### Governance for upgrading:

1. The team will initiate a proposal for the upgrade (optional)
2. All nodes will have to complete upgrading by the stipulated block height
3. The nodes who have not upgraded by then will not be part of the consensus and if your validator node experiences [too long a downtime, it will be jailed and slashed](../../validators/validator-faq.md#what-are-the-slashing-conditions).
4. There will be a governance proposal initiated after to initialize and affirm this upgrade
5. After the proposal is passed, the EVM module will automatically start to run and the corresponding port 8545 will start. At this stage, the validator does not need to do anything except whether to vote in the proposal.
