# Binaries - Upgrading Your Node

### f(x)Core Network Upgrades

> For more information on past upgrades and instructions, refer to [**Upgrade Versions**](../versions/README.md).
>
> You may refer to this [**Countdown Timer**](https://starscan.io/fxcore/block/countdown/16838000?chainId=fxcore) which will countdown the time till the upgrade height.

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

2\. Get the latest `fxcored` binary

Pulling the latest fx-core code base (ensure that you are in the fx-core folder):

```
git pull
```

Checkout the branch of the upgrade version:

```shell
git checkout <upgradeable version branch>
```

for example:

```
git checkout release/v7.5.x

or

git checkout tags/v7.5.0
```

Update fxcored (ensure that you are in the fx-core folder):

```
make install
```

Cross reference the latest commit hash to the commit in our [official github page](https://github.com/FunctionX/fx-core):

```
fxcored version
```

3\. Update config files

```
fxcored config update
```

4\. Restart the node:

```
sudo systemctl restart fxcored
```

5\. Check whether the node is participating in consensus:

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
