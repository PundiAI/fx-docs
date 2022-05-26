# Testnet faucet

### Request Funds

1. To request for Testnet FX click on this [link](https://testnet-faucet.functionx.io/)
2. Input your address (be sure to choose the correct wallet type for the network you wish to receive funds in)
3. Choose from the dropdown the appropriate network and coin/token type

![Testnet Faucet](<../.gitbook/assets/image (29).png>)

### Viewing your wallet balances

* You may view your balance on f(x)wallet which also allows for a more seamless way of transaction. You may download f(x)wallet from the Appstore or Google Play [here](https://download.functionx.io).
* Once downloaded, enter into the settings configuration, (the button is on the top right, there is a settings icon)
  * Under general/Network Configuration, ensure your f(x)Core is toggled to `Testnet`
* You may view your FX balance from the f(x) Wallet or the [Function X Testnet Explorer](https://dhobyghaut-explorer.functionx.io/)/[Function X Mainnet Explorer](https://explorer.functionx.io/).
* Alternatively, to `query` your validator account balance (token holding account):

```bash
fxcored q bank balances <token holding account public address>
```
