---
description: >-
  Truffle is a world-class development environment, testing framework and asset
  pipeline for blockchains using the Ethereum Virtual Machine (EVM), aiming to
  make life as a developer easier.
---

# With Truffle

### **Setting up the development environment**

There are a few dependencies to install before we start. Please install the following:

* [Node.js v8+ LTS and npm](https://nodejs.org/en/download/) (comes with Node)
* [Git](https://git-scm.com)

{% hint style="info" %}
Do ensure that your npm modules have been added to your [path](https://www.java.com/en/download/help/path.html)
{% endhint %}

After installing the above dependencies, we can proceed to install truffle:

```
npm install -g truffle
```

> for more information on the different flags for [npm-install](https://docs.npmjs.com/cli/v8/commands/npm-install)

To verify that Truffle is installed properly, run this command:

```
truffle version
```

Return (your version might differ):

```
Truffle v5.4.6 (core: 5.4.6)
Solidity v0.5.16 (solc-js)
Node v17.3.0
Web3.js v1.5.1
```

> If you're new to Truffle then please follow the [Getting Started](https://www.trufflesuite.com/docs/truffle/quickstart) by truffle, To setup the truffle environment.

### Initializing a truffle project

Initialize a truffle project:

```
truffle init
```

Initialize a node project:

```
npm init -y
```

### **truffle-config**

* After setting up your truffle project, be it cloning a repository or initializing a new truffle project
* Open `truffle-config.js`
* Edit `truffle-config.js` with f(x)Core network credentials

> the file below is just a sample:

{% code title="truffle-config.js" %}
```jsdoc
require('babel-register');
require('babel-polyfill');
const HDWalletProvider = require('@truffle/hdwallet-provider');
const fs = require('fs');
const mnemonic = fs.readFileSync(".secret").toString().trim();


module.exports = {
    // See <http://truffleframework.com/docs/advanced/configuration>
    // to customize your Truffle configuration!
    // contracts_build_directory: path.join(__dirname, "client/src/contracts"),
    networks: {
        development: {
            host: "127.0.0.1",
            port: 7545,
            network_id: "*" // Match any network id
        },
        fxtestnet: {
            provider: () => new HDWalletProvider(mnemonic, 'https://testnet-fx-json-web3.functionx.io:8545'),
            gasPrice: 4000000000000,   //this is 4000 Gwei
            network_id: 90001
        },

    },

    contracts_directory: './src/contracts/',
    contracts_build_directory: './src/abis/',
    compilers: {

        solc: {
            version: "0.8.0",
            settings: {

                optimizer: {
                    enabled: true,
                    runs: 200
                },
                evmVersion: "petersburg"
            }
        }
    }
};
```
{% endcode %}

{% hint style="warning" %}
The key fields to note here are:

* networks to include fxtestnet
* provider to be set to any of our f(x)Core nodes, you may use the company's hosted node https://testnet-fx-json-web3.functionx.io:8545
* gasPrice: 4000000000000
* network\_id: 90001
{% endhint %}

{% hint style="info" %}
Notice, it requires mnemonic to be passed in for fxtestnet Provider, this is the seed phrase for the account you'd like to deploy from. Create a new .secret file in root directory and enter your 12 word mnemonic seed phrase to get started. To get the seed phrase from metamask wallet you can click into the Metamask icon-->Metamask Settings-->Security & Privacy-->Reveal Secret Recovery Phrase.
{% endhint %}

### **Deploying on f(x)Core Network**

To request for Testnet FX click on this [link](https://dhobyghaut-faucet.functionx.io)

Run this command in root of the project directory:

```
truffle migrate --network fxtestnet
```

Contract will be deployed on f(x)Core Testnet, it will look something like this:

```
2_deploy_contracts.js
=====================

   Replacing 'MyContract'
   -------------------
   > transaction hash:    0x1be00a123a4751b25c71bf953a768051f5c518d21863637caa2d52b81bb8d44b
   > Blocks: 1            Seconds: 5
   > contract address:    0x5e7aC33b4811309bbeAe43212CA0719C450a2EaE
   > block number:        551956
   > block timestamp:     1640168723
   > account:             0x2a9830C640d84fCeA5DF266cfEaA33428937F265
   > balance:             1892.181296
   > gas used:            255717 (0x3e6e5)
   > gas price:           4000 gwei
   > value sent:          0 ETH
   > total cost:          1.022868 ETH

deploy done

   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:            1.022868 ETH


Summary
=======
> Total deployments:   2
> Final cost:          1.567316 ETH
```

> Remember your address, transaction\_hash and other details provided would differ, Above is just to provide an idea of structure.

**Congratulations!** You have successfully deployed HelloWorld Smart Contract. Now you can interact with the Smart Contract.

You can check the deployment status in our [testnet explorer](https://testnet-fxscan.functionx.io).
