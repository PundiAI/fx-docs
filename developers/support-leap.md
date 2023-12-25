# Leap

Add [Suggest Chain](https://docs.leapwallet.io/cosmos/for-dapps-connect-to-leap/suggest-chain-add-leap-to-a-non-native-chain)

1. [Install Leap browser extension.](https://www.leapwallet.io/cosmos)
2. Open the browser console
3. Run the code below

> The same network (with the same chainId) can only add one

{% tabs %}
{% tab title="Mainnet" %}
```javascript
await window.leap.experimentalSuggestChain({
  chainId: "fxcore",
  chainName: "f(x)Core",
  rpc: "https://fx-json.functionx.io:26657",
  rest: "https://fx-rest.functionx.io",
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
    },
  ],
  feeCurrencies: [
    {
      coinDenom: "FX",
      coinMinimalDenom: "FX",
      coinDecimals: 18,
      coinGeckoId: "fx-coin",
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
  }, 
  image: "https://raw.githubusercontent.com/cosmos/chain-registry/master/fxcore/images/fx.svg",
});
```
{% endtab %}

{% tab title="Testnet" %}
```javascript
await window.leap.experimentalSuggestChain({
  chainId: "dhobyghaut",
  chainName: "f(x)Core Testnet",
  rpc: "https://testnet-fx-json.functionx.io:26657",
  rest: "https://testnet-fx-rest.functionx.io",
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
    },
  ],
  feeCurrencies: [
    {
      coinDenom: "FX",
      coinMinimalDenom: "FX",
      coinDecimals: 18,
      coinGeckoId: "fx-coin",
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
  },
  image: "https://raw.githubusercontent.com/cosmos/chain-registry/master/fxcore/images/fx.svg",
});
```
{% endtab %}
{% endtabs %}
