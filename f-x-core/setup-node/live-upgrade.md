# Live Upgrade

This document demonstrates how a live upgrade can be performed on-chain through a governance process.

1.  Start the network and set in motion an upgrade proposal

    ```bash
    # start a f(x)Core application full-node
    $ fxcored start

    # set up the cli config
    $ fxcored config trust-node true
    $ fxcored config chain-id testing

    # create an upgrade governance proposal
    $ fxcored tx gov submit-proposal software-upgrade <plan-name> \
    --title <proposal-title> --description <proposal-description> \
    --from <name-or-key> --upgrade-height <desired-upgrade-height> --deposit 10000000000000000000000FX

    # once the proposal passes you can query the pending plan
    $ fxcored query upgrade plan
    ```
2.  Performing an upgrade

    Assuming the proposal passes, the chain will stop at the given upgrade height.

    You can stop and start the original binary if you want, but **it will refuse to run after the upgrade height**.

    We need a new binary with the upgrade handler installed. The logs should look something like:

    ```bash
    E[2019-11-05|12:44:18.913] UPGRADE "<plan-name>" NEEDED at height: <desired-upgrade-height>:       module=main
    E[2019-11-05|12:44:18.914] CONSENSUS FAILURE!!!
    ...Note that the process will hang indefinitely (doesn't exit to avoid restart loops). So, you must manually kill the process and replace it with a new binary. Do so now with `Ctrl+C` or `killall fxcored`.
    ```

````
In `fxcore/app/app.go`, after `upgrade.Keeper` is initialized and set in the app, set the corresponding upgrade `Handler` with the correct `<plan-name>`:

```go
    app.upgradeKeeper.SetUpgradeHandler("<plan-name>", func(ctx sdk.Context, plan upgrade.Plan) {
        // custom logic after the network upgrade has been executed
    })
```
````

{% hint style="info" %}
Note that we need not panic at the first sign of an error - an upgrade would fail if the migration did not follow through, and no node would advance - resulting in a manual recovery. If we ignored the errors, we would proceed with an incomplete upgrade and have a very difficult time trying to recover the proper state.
{% endhint %}

Now, compile the new binary and run the upgraded code to complete the upgrade:

```
# create a new binary of f(x)Core with the added upgrade handler
$ make install

# Restart the chain using the new binary. You should see the chain resume from
# the upgrade height:
# `I[2019-11-05|12:48:15.184] applying upgrade <plan-name> at height: <desired-upgrade-height>      module=main`
$ fxcored start

# verify there is no pending plan
$ fxcored query upgrade plan

# verify you can query the block header of the completed upgrade
$ fxcored query upgrade applied <plan-name>
```

For other software upgrades (make sure you are in the `fx-core` directory) run these commands:

```bash
# this will ensure you have the latest code stack
git pull

# the following commands will upgrade your binaries
make go.sum
make install
```
