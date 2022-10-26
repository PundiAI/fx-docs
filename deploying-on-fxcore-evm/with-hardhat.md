---
description: >-
  Hardhat is a development environment to compile, deploy, test, and debug your
  Ethereum software.
---

# With Hardhat

### **Setting up the development environment**

There are a few dependencies to install before we start. Please install the following:

* [Node.js v8+ LTS and npm](https://nodejs.org/en/download/) (comes with Node)
* [Git](https://git-scm.com/)

{% hint style="info" %}
Do ensure that your npm modules have been added to your [path](https://www.java.com/en/download/help/path.html)
{% endhint %}

After installing the above dependencies, we can proceed to install hardhat:

```
npm install --save-dev hardhat
```

To create your Hardhat project, run this command while you are in the project's root folder:

```
npx hardhat
```

Return (choose the most appropriate option):

```
? What do you want to do? ... 
> Create a basic sample project
  Create an advanced sample project
  Create an advanced sample project that uses TypeScript
  Create an empty hardhat.config.js
  Quit
```

Let’s create the sample project and go through these steps to try out the sample task and compile, test and deploy the sample contract.

The sample project will ask you to install `hardhat-waffle` and `hardhat-ethers`, which makes Hardhat compatible with tests built with Waffle. You can learn more about it [in this guide](https://hardhat.org/guides/waffle-testing.html).

> Hardhat will let you know how, but, in case you missed it, you can install them with `npm install --save-dev @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers`

### **hardhat-config**

* After setting up your truffle project, be it cloning a repository or initializing a new hardhat project
* Open `hardhat-config.js`
* Edit `hardhat-config.js` with f(x)Core network credentials
* Create a .env file to store your private key of that corresponding wallet address

> The files below are just an example, feel free to rename your variables:

{% code title=".env" %}
```
FXCORE_WALLET_PRIVATE_KEY=c3b8df3c3165067e05XXXXXXXXXXXXXXXXXXXXXXXXxxxxXXXXXXbb
```
{% endcode %}

{% code title="hardhat.config.js" %}
```
require("@nomiclabs/hardhat-waffle");
require("dotenv").config();
const fxcorePrivateKey=process.env.FXCORE_WALLET_PRIVATE_KEY

module.exports = {
  defaultNetwork: "fxtestnet",
  networks: {
    hardhat: {
    },
    fxtestnet: {
      url: "https://testnet-fx-json-web3.functionx.io:8545",
      accounts: [`0x${fxcorePrivateKey}`]
    }
  },
  solidity: {
    version: "0.8.0",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200
      }
    }
  },
}

```
{% endcode %}

{% hint style="info" %}
if you have not already installed dontenv, please do so by running the following command:

```
npm install dotenv 
```
{% endhint %}

### **Compile Smart contract file**

```
npx hardhat compile
```

### **Deploying on f(x)Core Network**

Run this command in root of the project directory:

```
npx hardhat run scripts/sample-script.js --network fxtestnet
```

Contract will be deployed on f(x)Core network, it will look something like this:

```
Greeter deployed to: 0x70A8aa0fb6a0B4732b2daaBc3143E4C3d560DD57
```

> Remember your address would differ, the above is just to provide an idea of structure. **Congratulations!** You have successfully deployed Greeter Smart Contract. Now you can interact with the Smart Contract.

You can check the deployment status on [Testnet](https://testnet-fxscan.functionx.io/).
