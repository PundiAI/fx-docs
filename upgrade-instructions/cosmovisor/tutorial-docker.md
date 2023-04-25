# Cosmovisor Integration - Docker

{% hint style="warning" %}
❗️Please note that the current fxCore testnet v4 upgrade, the upgrade height is `8088000`
{% endhint %}

> For more information on past upgrades and instructions, refer to [**Upgrade Versions**](../versions/README.md).
>
> You may refer to this [**Countdown Timer**](https://functionx.github.io/fx-core/tools/countdown.html?network=testnet) which will countdown the time till the upgrade height.

Docker image already has cosmovisor, just pull the image and run it

```sh
docker pull ghcr.io/functionx/fxcorevisor:3.1.0
# sto and remove the old container
docker stop fxcore && docker rm fxcore
# run the new container with cosmovisor
docker run --name fxcore -d --restart=always -p 0.0.0.0:26656:26656 -p 127.0.0.1:26657:26657 -p 127.0.0.1:1317:1317 -p 127.0.0.1:26660:26660 -p 127.0.0.1:8545:8545 -p 127.0.0.1:8546:8546 -v $HOME/.fxcore:/root/.fxcore ghcr.io/functionx/fxcorevisor:3.1.0 run start --x-crisis-skip-assert-invariants
```