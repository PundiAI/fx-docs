## Minting your Testnet FX

* The FX-testnet faucet: https://aabbcc-fx-rest.functionx.io/other/faucet/<your wallet address>
* Copy and paste the url above in your browser search bar and replace the wallet address to that of your own
* It will look something like this https://aabbcc-fx-rest.functionx.io/other/faucet/fx12u8erfqdd75r4apyqvpxst63w0n3wvr2asncf5
* Each time you run the search, it will mint 100FX into your wallet address
* The current versions of `f(x) Wallet` does not support `fx-core` testnet so you will have to download the beta version
    * The `Appstore` [version](http://releaseapp.functionx.io/2v51gx). To entrust this [enterprise developer](https://entirefaq.helpdocs.com/mobile-apps/iphone-app-how-to-fix-the-untrusted-enterprise-developer)
    * The `Android` [version](http://releaseapp.functionx.io/ph54e5)
* You may view your FX balance from the f(x) Wallet
* The name of the `Ethereum Testnet FX` is `FX-TEST` (the correct asset must be added to your `f(x) Wallet`)
* To `query` your validator account balance (token holding account):
```bash
fxcored q bank balances <token holding account public address>
```