# Cosmovisor Integration - Docker

Docker image already has cosmovisor, just pull the image and run it

```
docker pull ghcr.io/functionx/fxcorevisor:3.1.0
docker stop fxcore && docker rm fxcore
docker run --name fxcore -d --restart=always -p 26656:26656 -p 26657:26657 -p 1317:1317 -p 26660:26660 -p 8545:8545 -p 8546:8546 -v $HOME/.fxcore:/root/.fxcore ghcr.io/functionx/fxcorevisor:3.1.0 run start --x-crisis-skip-assert-invariants
```