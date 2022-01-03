---
description: >-
  Remix IDE is an open source web and desktop application. It fosters a fast
  development cycle and has a rich set of plugins with intuitive GUIs.
---

# With Remix

A Hello World style starter project. Deploys a smart contract with a message, and renders it in the front-end. You can change the message using the interactive panel!

This DAPP implements a "Hello World" style application that echoes a message passed to the contract to the front end. This tutorial is intended to be followed using the online IDE available at [Remix IDE](https://remix.ethereum.org).

For more information on Remix and how to use it, you may find it in the [Remix Documentation](https://remix-ide.readthedocs.io/en/latest/index.html).

### Setting up [Remix IDE](https://remix.ethereum.org)

* Remix IDE - an online IDE to develop smart contracts.
* If you’re new to Remix, you’ll first need to activate two modules: **Solidity Compiler** and **Deploy and Run Transactions** (This should already be activated by default without you needing to search for it in the plugin manager).
* Search for **'Solidity Compiler'** in the plugin tab in Remix (this should be activated by default)
* And activate the plugins (if they are not already activated)

![](<../.gitbook/assets/image (18) (1).png>)![](<../.gitbook/assets/image (22).png>)

* The environment should be set to solidity by default

{% tabs %}
{% tab title="HelloWorld Tutorial" %}
* ![](<../.gitbook/assets/image (14).png>)Go to File Explorers, ![](<../.gitbook/assets/image (29) (1).png>)To create a new file , Name it HelloWorld.sol
* **Copy/Paste** the Smart contract below into the newly created file `HelloWorld.sol`

{% code title="HelloWorld.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
// Specifies that the source code is for a version
// of Solidity greater than 0.8.10

// A contract is a collliection of functions and data (its state)
// that resides at a specific address on the Ethereum blockchain.
contract HelloWorld {

    // The keyword "public" makes variables accessible from outside a contract
    // and creates a function that other contracts or SDKs can call to access the value
    string public message;

    // A special function only run during the creation of the contract
    constructor(string memory initMessage) {
        // Takes a string value and stores the value in the memory data storage area,
        // setting `message` to that value
        message = initMessage;
    }

    // A publicly accessible function that takes a string as a parameter
    // and updates `message`
    function update(string memory newMessage) public {
        message = newMessage;
    }
}
```
{% endcode %}

The first line, `pragma solidity ^0.8.0` specifies that the source code is for a Solidity version greater than 0.8.0. [Pragmas](https://solidity.readthedocs.io/en/latest/layout-of-source-files.html#pragma) are common instructions for compilers about how to treat the source code (e.g., pragma once).

A contract in the sense of Solidity is a collection of code (its functions) and data (its state) that resides at a specific address on the Ethereum blockchain. The line `string public message` declares a public state variable called `message` of type `string`. You can think of it as a single slot in a database that you can query and alter by calling functions of the code that manages the database. The keyword public automatically generates a function that allows you to access the current value of the state variable from outside of the contract. Without this keyword, other contracts have no way to access the variable.

The [constructor](https://solidity.readthedocs.io/en/latest/contracts.html#constructor) is a special function run during the creation of the contract and cannot be called afterward. In this case, it takes a string value `initMessage`, stores the value in the [memory](https://solidity.readthedocs.io/en/latest/introduction-to-smart-contracts.html#storage-memory-and-the-stack) data storage area, and sets `message` to that value.

The `string public message` function is another public function that is similar to the constructor, taking a string as a parameter, and updating the `message` variable.
{% endtab %}

{% tab title="ERC20 Tutorial" %}
* ![](<../.gitbook/assets/image (14).png>)Go to File Explorers, ![](<../.gitbook/assets/image (29) (1).png>)To create a new file , Name it erc20.sol
* **Copy/Paste** the Smart contract below into the newly created file `erc20.sol`
* **Edit** the `string`` `<mark style="color:blue;">`public`</mark>` ``name` & `string`` `<mark style="color:blue;">`public`</mark>` ``symbol` and replace it with your own name and symbol

{% hint style="info" %}
Feel free to edit other parts of the contract. This is just a sample ERC20 contract for testing and might not have the necessary security checks in place.
{% endhint %}

{% code title="erc20.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Token {
    string public name = "Bluestitch";
    string public symbol = "WalaoBTC";
    uint256 public totalSupply;
    uint8 public decimals = 18;

    event Transfer(address indexed _from, address indexed _to, uint256 _value);

    event Approval(
        address indexed _owner,
        address indexed _spender,
        uint256 _value
    );

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    constructor() {
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address _to, uint256 _value)
        public
        returns (bool success)
    {
        require(balanceOf[msg.sender] >= _value);
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value)
        public
        returns (bool success)
    {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) public returns (bool success) {
        require(_value <= balanceOf[_from]);
        require(_value <= allowance[_from][msg.sender]);
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }

    function _mint(address account, uint256 amount) public {
        require(account != address(0), "ERC20: mint to the zero address");
        totalSupply += amount;
        balanceOf[account] += amount;
        emit Transfer(address(0), account, amount);
    }
}

```
{% endcode %}

The first line, `pragma solidity ^0.8.0` specifies that the source code is for a Solidity version greater than 0.8.0. [Pragmas](https://solidity.readthedocs.io/en/latest/layout-of-source-files.html#pragma) are common instructions for compilers about how to treat the source code (e.g., pragma once).

A contract in the sense of Solidity is a collection of code (its functions) and data (its state) that resides at a specific address on the Ethereum blockchain. The line `string public name & symbol` declares public state variables called `name & symbol` of type `string`. The line `uint256`` `<mark style="color:blue;">`public`</mark>` ``totalSupply` declares a uint256 variable that stores the amount of this particular token in existence. You can think of it as a single slot in a database that you can query and alter by calling functions of the code that manages the database. The keyword public automatically generates a function that allows you to access the current value of the state variable from outside of the contract. Without this keyword, other contracts have no way to access the variable.

Events are called Events because they are very good at signalling that an event has taken place. In blockchain terms, they’re cheap. They’re cheap to write in a contract in terms of deployment cost and gas cost when calling a function with an event, and they’re free to read. In short, they’re a very good way of broadcasting relevant information.

For more information on the ERC20 standard and the various variables, you may refer [here](https://docs.openzeppelin.com/contracts/2.x/api/token/erc20).
{% endtab %}
{% endtabs %}

### Compile Smart Contract

* ![](<../.gitbook/assets/image (28).png>) Go to Solidity Compiler
* Select Compiler Version to 0.8.0
* Now, `Compile HelloWorld.sol` /`ERC20.sol`
* After Successful Compilation, it will show ![](<../.gitbook/assets/image (24).png>)
* Now, we have to deploy our smart contract on f(x)Core Network. For that, we have to connect to web3, this can be done by using services like Metamask. We will be using Metamask. Please follow this tutorial to setup a Metamask Account.
* Open Metamask, click the **network dropdown** and then click **'Add Network'**

![](<../.gitbook/assets/image (25).png>)

* Put in a Network name (just an example):

```
fxtothemoon
```

* In New RPC URL field you can add the URL:

```
https://testnet-fx-json-web3.functionx.io:8545
```

* Enter the Chain ID:

```
90001
```

* (Optional Field) Currency Symbol (just an example):

```
FX
```

* (Optional Field) Block Explorer URL:&#x20;

```
https://testnet-fxscan.functionx.io/
```

![Metamask Network Configuration](<../.gitbook/assets/image (3).png>)

* Click Save
* Copy your address from Metamask

![](<../.gitbook/assets/image (27).png>)

* Head over to [faucet](https://dhobyghaut-faucet.functionx.io) and request test FX - you will need this to pay for gas on f(x)Core. After inputting your wallet address in, select the option **'100 (fxCore) FX / 24h'**.
* Now, let's Deploy the Smart Contract to the f(x)Core Network
* Select Injected Web3 in the Environment dropdown ensure you have selected the right contract too.
* Accept the connection request by clicking **Next** in Metamask after choosing the account

![Injected Web3](<../.gitbook/assets/image (1).png>)

* Once Metamask is connected to Remix, the ‘Deploy’ transaction would generate another metamask popup that requires transaction confirmation.
* Click the <mark style="color:blue;">**EDIT**</mark> button (1st picture) and then the <mark style="color:orange;">**Edit suggested gas fee**</mark> (2nd picture) before editing the **Max priority fee** and **Max fee** to 4000 Gwei then click **SAVE**.

![Adjusting Gas Price](<../.gitbook/assets/image (20).png>)

* Click **Confirm**

![](<../.gitbook/assets/image (7).png>)

**Congratulations!** You have successfully deployed HelloWorld/ERC20 Smart Contract. Now you can interact with the Smart Contract. Check the deployment status [here](https://testnet-fxscan.functionx.io).

![Interacting with Deployed Contract](<../.gitbook/assets/image (5) (1).png>)

### ERC20 Tutorial Extended

After deploying the `ERC20.sol`, your Remix should look something like the following:

![ERC20 Remix Page](<../.gitbook/assets/image (2).png>)

Taking a look at the side panel in particular these sets of button functions where you can interact with the contract. By clicking on the drop down for those buttons that have a dropdown will require that you fill in those fields before clicking on the button to query/write the value of the function. For those buttons that do not have a field to fill in, you may just click on the button to read/write.

<mark style="color:orange;">Orange buttons</mark> are **writeable** button functions. <mark style="color:blue;">Blue buttons</mark> are readable button functions

![Remix Button Functions](<../.gitbook/assets/image (17).png>)

Copy the ![](<../.gitbook/assets/image (18).png>)deployed contract address and input it in Metamask. And step through with adding the custom token.

![](<../.gitbook/assets/image (30).png>)![](<../.gitbook/assets/image (16).png>)

Now with our custom token added, we are all ready to mint some tokens and interact with the ERC20 contract.

![](<../.gitbook/assets/image (5).png>)

Clicking into the \_mint (example) dropdown, you will be shown a few fields:

![](<../.gitbook/assets/image (10).png>)

The fields are pretty self explanatory. Amount has to be of the type unit256 (unsigned integer), while Account has to be of the address (0x) type. Input your address and the amount you would like to mint. Do remember to add 18 0s behind. The amount value here is expressed in Wei. So if you want to mint 100 of your tokens, the field should be 100000000000000000000.

![](<../.gitbook/assets/image (29).png>)![](../.gitbook/assets/image.png)

Hitting ![](<../.gitbook/assets/image (31).png>) will result in a Metamask pop-up. Remember to edit the gas fields to reflect 4000 GasPrice similar to what you did before and voila, you will have minted some of your own tokens. With your own minted tokens, you can play around with some of [our protocols](deployed-testnet-dapps.md) especially Uniswap.

![](<../.gitbook/assets/image (4).png>)

Now lets go through the rest of the buttons one by one:

<details>

<summary>totalSupply() → uint256</summary>

Returns the amount of tokens in existence.

</details>

<details>

<summary>symbol() → string</summary>

Returns the symbol of the token, usually a shorter version of the name.

</details>

<details>

<summary>name() → string</summary>

Returns the name of the token.

</details>

<details>

<summary>decimals() → uint8</summary>

Returns the number of decimals used to get its user representation. For example, if `decimals` equals `2`, a balance of `505` tokens should be displayed to a user as `5,05` (`505 / 10 ** 2`).

Tokens usually opt for a value of 18, imitating the relationship between Ether and Wei.

</details>

<details>

<summary>balanceOf(address account) → uint256</summary>

Returns the amount of tokens owned by `account`.

Do not forget to fill in the `account` field with the 0x address you would like to query.

</details>

<details>

<summary>allowance(address owner, address spender) → uint256</summary>

Returns the remaining number of tokens that `spender` will be allowed to spend on behalf of `owner` through [`transferFrom`](https://docs.openzeppelin.com/contracts/2.x/api/token/erc20#IERC20-transferFrom-address-address-uint256-). This is zero by default.

This value changes when [`approve`](https://docs.openzeppelin.com/contracts/2.x/api/token/erc20#IERC20-approve-address-uint256-) or [`transferFrom`](https://docs.openzeppelin.com/contracts/2.x/api/token/erc20#IERC20-transferFrom-address-address-uint256-) are called.

</details>

<details>

<summary>transferFrom(address sender, address recipient, uint256 amount) → bool</summary>

Moves `amount` tokens from `sender` to `recipient` using the allowance mechanism. `amount` is then deducted from the caller’s allowance.

Returns a boolean value indicating whether the operation succeeded.

Emits a [`Transfer`](https://docs.openzeppelin.com/contracts/2.x/api/token/erc20#IERC20-Transfer-address-address-uint256-) event.

</details>

<details>

<summary>transfer(address recipient, uint256 amount) → bool</summary>

Moves `amount` tokens from the caller’s account to `recipient`.

Returns a boolean value indicating whether the operation succeeded.

Emits a [`Transfer`](https://docs.openzeppelin.com/contracts/2.x/api/token/erc20#IERC20-Transfer-address-address-uint256-) event.

</details>

<details>

<summary>approve(address spender, uint256 amount) → bool</summary>

Sets `amount` as the allowance of `spender` over the caller’s tokens.

Returns a boolean value indicating whether the operation succeeded.

Emits an [`Approval`](https://docs.openzeppelin.com/contracts/2.x/api/token/erc20#IERC20-Approval-address-address-uint256-) event.

</details>

<details>

<summary>_mint(address account, uint256 amount)</summary>

Creates `amount` tokens and assigns them to `account`, increasing the total supply.

Emits a [`transfer`](https://docs.openzeppelin.com/contracts/2.x/api/token/erc20#ERC20-transfer-address-uint256-) event with `from` set to the zero address.

Requirements

* `to` cannot be the zero address.

</details>
