# Keplr

Add [Suggest Chain](https://docs.keplr.app/api/suggest-chain.html)

1. [Install Keplr browser extension.](https://www.keplr.app/download)
2. Open the browser console
3. Run the code below

> The same network (with the same chainId) can only add one

{% tabs %}
{% tab title="Mainnet" %}
```javascript
await window.keplr.experimentalSuggestChain({
  chainId: "fxcore",
  chainName: "f(x)Core",
  rpc: "https://fx-json.functionx.io:26657",
  rest: "https://fx-rest.functionx.io",
  walletUrl: "https://starscan.io/fxcore/validators",
  walletUrlForStaking: "https://starscan.io/fxcore/validators",
  bip44: {
    coinType: 60,
  },
  bech32Config: {
    bech32PrefixAccAddr: "fx",
    bech32PrefixAccPub: "fxpub",
    bech32PrefixValAddr: "fxvaloper",
    bech32PrefixValPub: "fxvaloperpub",
    bech32PrefixConsAddr: "fxvalcons",
    bech32PrefixConsPub: "fxvalconspub",
  },
  currencies: [
    {
      coinDenom: "FX",
      coinMinimalDenom: "FX",
      coinDecimals: 18,
      coinGeckoId: "fx-coin",
      coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
    },
  ],
  feeCurrencies: [
    {
      coinDenom: "FX",
      coinMinimalDenom: "FX",
      coinDecimals: 18,
      coinGeckoId: "fx-coin",
      coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
      gasPriceStep: {
        low: 4000000000000,
        average: 4000000000000,
        high: 4100000000000,
      },
    },
  ],
  stakeCurrency: {
    coinDenom: "FX",
    coinMinimalDenom: "FX",
    coinDecimals: 18,
    coinGeckoId: "fx-coin",
    coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
  },
  features: ["eth-address-gen", "eth-key-sign", "ibc-transfer"],
  beta: true,
});
```
{% endtab %}

{% tab title="Mainnet Classic" %}
```javascript
await window.keplr.experimentalSuggestChain({
  chainId: "fxcore",
  chainName: "f(x)Core Classic",
  rpc: "https://fx-json.functionx.io:26657",
  rest: "https://fx-rest.functionx.io",
  walletUrl: "https://starscan.io/fxcore/validators",
  walletUrlForStaking: "https://starscan.io/fxcore/validators",
  bip44: {
    coinType: 118,
  },
  bech32Config: {
    bech32PrefixAccAddr: "fx",
    bech32PrefixAccPub: "fxpub",
    bech32PrefixValAddr: "fxvaloper",
    bech32PrefixValPub: "fxvaloperpub",
    bech32PrefixConsAddr: "fxvalcons",
    bech32PrefixConsPub: "fxvalconspub",
  },
  currencies: [
    {
      coinDenom: "FX",
      coinMinimalDenom: "FX",
      coinDecimals: 18,
      coinGeckoId: "fx-coin",
      coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
    },
  ],
  feeCurrencies: [
    {
      coinDenom: "FX",
      coinMinimalDenom: "FX",
      coinDecimals: 18,
      coinGeckoId: "fx-coin",
      coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
      gasPriceStep: {
        low: 4000000000000,
        average: 4000000000000,
        high: 4100000000000,
      },
    },
  ],
  stakeCurrency: {
    coinDenom: "FX",
    coinMinimalDenom: "FX",
    coinDecimals: 18,
    coinGeckoId: "fx-coin",
    coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
  },
});
```
{% endtab %}

{% tab title="Testnet" %}
```javascript
await window.keplr.experimentalSuggestChain({
  chainId: "dhobyghaut",
  chainName: "f(x)Core Testnet",
  rpc: "https://testnet-fx-json.functionx.io:26657",
  rest: "https://testnet-fx-rest.functionx.io",
  walletUrl: "https://testnet.starscan.io/fxcore/validators",
  walletUrlForStaking: "https://testnet.starscan.io/fxcore/validators",
  bip44: {
    coinType: 60,
  },
  bech32Config: {
    bech32PrefixAccAddr: "fx",
    bech32PrefixAccPub: "fxpub",
    bech32PrefixValAddr: "fxvaloper",
    bech32PrefixValPub: "fxvaloperpub",
    bech32PrefixConsAddr: "fxvalcons",
    bech32PrefixConsPub: "fxvalconspub",
  },
  currencies: [
    {
      coinDenom: "FX",
      coinMinimalDenom: "FX",
      coinDecimals: 18,
      coinGeckoId: "fx-coin",
      coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
    },
  ],
  feeCurrencies: [
    {
      coinDenom: "FX",
      coinMinimalDenom: "FX",
      coinDecimals: 18,
      coinGeckoId: "fx-coin",
      coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
      gasPriceStep: {
        low: 4000000000000,
        average: 4000000000000,
        high: 4100000000000,
      },
    },
  ],
  stakeCurrency: {
    coinDenom: "FX",
    coinMinimalDenom: "FX",
    coinDecimals: 18,
    coinGeckoId: "fx-coin",
    coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
  },
  features: ["eth-address-gen", "eth-key-sign", "ibc-transfer"],
  beta: true,
});
```
{% endtab %}

{% tab title="Testnet Classic" %}
```javascript
await window.keplr.experimentalSuggestChain({
  chainId: "dhobyghaut",
  chainName: "f(x)Core Testnet Classic",
  rpc: "https://testnet-fx-json.functionx.io:26657",
  rest: "https://testnet-fx-rest.functionx.io",
  walletUrl: "https://testnet.starscan.io/fxcore/validators",
  walletUrlForStaking: "https://testnet.starscan.io/fxcore/validators",
  bip44: {
    coinType: 118,
  },
  bech32Config: {
    bech32PrefixAccAddr: "fx",
    bech32PrefixAccPub: "fxpub",
    bech32PrefixValAddr: "fxvaloper",
    bech32PrefixValPub: "fxvaloperpub",
    bech32PrefixConsAddr: "fxvalcons",
    bech32PrefixConsPub: "fxvalconspub",
  },
  currencies: [
    {
      coinDenom: "FX",
      coinMinimalDenom: "FX",
      coinDecimals: 18,
      coinGeckoId: "fx-coin",
      coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
    },
  ],
  feeCurrencies: [
    {
      coinDenom: "FX",
      coinMinimalDenom: "FX",
      coinDecimals: 18,
      coinGeckoId: "fx-coin",
      coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
      gasPriceStep: {
        low: 4000000000000,
        average: 4000000000000,
        high: 4100000000000,
      },
    },
  ],
  stakeCurrency: {
    coinDenom: "FX",
    coinMinimalDenom: "FX",
    coinDecimals: 18,
    coinGeckoId: "fx-coin",
    coinImageUrl: "https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/images/fxcore/fx.png",
  },
})
```
{% endtab %}
{% endtabs %}
