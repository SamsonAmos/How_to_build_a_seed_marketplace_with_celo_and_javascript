# How to Build a Seed Marketplace dapp using Celo, Solidity and Javascript

## Table of Contents

  * [Introduction](#introduction)
    + [What is a Blockchain?](#what-is-a-blockchain?)
    + [What is Celo?](#what-is-celo?)
    + [What are Smart Contracts?](#what-are-smart-contracts?)
  * [Prerequisites](#prerequisites)
  * [Requirements](#requirements)
  * [Let us Begin](#let-us-begin)
  * [Developing Our Smart Contract](#developing-our-smart-contract)
  * [Deploying our smart contract](#deploying-our-smart-contract)
  * [Developing the frontend](#developing-the-frontend)
    + [Editing the index.html file](#editing-the-indexhtml-file)
    + [Editing the marketplace.sol and the marketplace.abi.json file](#editing-the-marketplacesol-and-the-marketplaceabijson-file)
    + [Editing the main.js file](#editing-the-mainjs-file)
  * [Deployment on Github pages](#deployment-on-github-pages)
  * [Conclusion](#conclusion)
  * [Next step](#next-step)
  * [About the Author](#about-the-author)

## Introduction:

Before we proceed here is a preview and a live demo link of what will be building:

[demo](https://samsonamos.github.io/AgroCelo1/)

Below is a preview of what we are going to build.

![image](images/1.PNG)

### What is a Blockchain?

A blockchain is a distributed database or ledger that is shared among the nodes of a computer network. As a database, a blockchain stores information electronically in digital format. Blockchains are best known for their crucial role in maintaining a secure and decentralized record of transactions. The innovation with a blockchain is that it guarantees the fidelity and security of a record of data and generates trust without the need for a trusted third party.

### What is Celo?

Celo is a blockchain protocol that aims to facilitate a global payments infrastructure by leveraging phone numbers. The protocol uses phone numbers as a proxy for public keys. It also uses it for issuing native tokens, thereby making the financial activity accessible to anyone globally. Celo also allows the development of smart contracts and decentralised applications on its blockchain. Users can use this feature and contribute to initiatives like Universal Basic Income and other social causes.

### What are Smart Contracts?

Smart contracts are simply programs stored on a blockchain that run when predetermined conditions are met. They typically are used to automate the execution of an agreement so that all participants can be immediately certain of the outcome, without any intermediary???s involvement or time loss. They can also automate a workflow, triggering the next action when conditions are met.

## Prerequisites:
In order for us to move further, you will need to have a basic understanding of the following:

- Basic understanding of blockchain concepts. You can click [here](https://dacade.org/communities/blockchain/courses/intro-to-blockchain) to learn.
- Basic understanding of what a smart contract is.
- Basic knowledge on Solidity and its concepts. you can click [here](https://dacade.org/communities/ethereum/courses/sol-101/learning-modules/dcc5e8e2-bc22-49a6-ace7-23ec7fcc81d5) to learn
- Basic knowledge of HTML and Javascript.
- Basic understanding of the [command line](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Command_line).

## Requirements: 

- Access to a computer with an internet connection and a chrome web browser.
- **[NodeJS](https://nodejs.org/en/download)** from V12.or higher
- A code editor or text editor. **[VSCode](https://code.visualstudio.com/download)** is recommended
- A terminal. **[Git Bash](https://git-scm.com/downloads)** is recommended
- Remix IDE. Click **[here](https://remix.ethereum.org)** for the web version.
- Celo Extension Wallet. Click **[here](https://chrome.google.com/webstore/detail/celoextensionwallet/kkilomkmpmkbdnfelcpgckmpcaemjcdh?hl=en)** to download.
- A Github account.

## Let us Begin

## Developing Our Smart Contract:

Let's begin by building our first smart contract on Solidity using the Remix IDE. The Remix IDE is a web based IDE that allows developers to write, test and deploy smart contracts on the blockchain. But for this tutorial we will be deploying our smart contract on the Celo network or blockchain.  

You can learn how the remix works by following the steps below:

  1. Go to https://remix.ethereum.org/.
  2. Click on the featured plugin, ???LEARNETH???.
  3. Click on Remix Basics.
  4. Start the tutorial and finish all lessons of Remix Basics.


Considering you have understood how the Remix IDE works, let's build our smart contract by create a Soliidity file: called `AgroCelo.sol`

  - Go to https://remix.ethereum.org/, 
  - Create a new file, 
  - Name it `AgroCelo.sol`. You can give it any name you want but let's stick to `AgroCelo.sol`.  The `.sol` extension indicates that it is a Solidity file.


On the first line of your `AgroCelo.sol` let's include a statement that specifies the license under which the code is being released.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity >=0.7.0 <0.9.0;
```

This license governs how the code can be used, and it is important to ensure that the correct license is used to avoid any legal issues. A resource such as SPDX can be used to help identify a suitable license.

Using the pragma keyword, you specify the Solidity version that you want the compiler to use. In this case, it should be higher than or equal to seven and lower than or equal to nine. It is important to specify the version of the compiler because Solidity changes constantly.

Next up, we define an `IERC20Token` interface which enables us to interact with the Celo USD stablecoin (cUSD). 

```solidity
// SPDX-License-Identifier: MIT

pragma solidity >=0.7.0 <0.9.0;

interface IERC20Token {
  function transfer(address, uint256) external returns (bool);
  function approve(address, uint256) external returns (bool);
  function transferFrom(address, address, uint256) external returns (bool);
  function totalSupply() external view returns (uint256);
  function balanceOf(address) external view returns (uint256);
  function allowance(address, address) external view returns (uint256);

  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed owner, address indexed spender, uint256 value);
}
```

ERC-20 tokens are a widely-used standard for creating digital assets on the Ethereum blockchain, and the cUSD is one of them.

These tokens have pre-defined functions and events that can be easily used in contracts and do not require any additional implementation. For example, you will be using the ERC-20 token's interface to interact with it, so that your contract can communicate with the token.

You can find more information on how to use these functions and events in the Celo **[documentation](https://docs.celo.org/developer-guide/celo-for-eth-devs)**. The documentation also provides more details on how to interact with ERC-20 tokens and how to use them with the Celo network.

After defining our `IERC20Token` interface`, we define our contract with the keyword contract and give it a name. In our case we used `AgroCelo` as our name. You can give it any name but ensure it's descriptive.

```solidity
contract AgroCelo{
    uint internal listedSeedLength = 0;
    address internal cUsdTokenAddress = 0x874069Fa1Eb16D44d622F2e0Ca25eeA172369bC1;
```

In the next line, you define a state variable named `listedSeedLength`, this is going to keep track of the number of seeds listed on the blockchain and also in storing `seedInformation`. It is of a `uint` type which means it can only store unsigned integer values. [(Learn more about data types in solidity)](https://docs.soliditylang.org/en/latest/types.html).

We also define the visibility of our variable as `internal` which means it cannot be accessed from external smart contracts or addresses and can only be modified within the smart contract. ([Learn more about visiblity](https://docs.soliditylang.org/en/latest/contracts.html#visibility-and-getters))

Next, To interact with the cUSD `ERC-20` token on the Celo alfajores test network, you need to know the address of the token.

After defining the single variables used in the contract, we need  to create a `model` for our seed called `SeedInformation` which you can give your own name if you wish. This model will take the basic details about a seed to be listed on the blockchain.

```solidity
// Ceating a struct to store seed details.
    struct SeedInformation {
        address  owner;
        string seedName;
        string seedImgUrl;
        string seedDetails;
        string  seedLocation;
        uint price;
        string email;
    }
```

To do this, you would require a struct data type with the keyword `struct` and give it multiple properties. ([Learn about structs here](https://docs.soliditylang.org/en/latest/types.html#structs))

For this tutorial, these would be the variables that you would store in the struct:
1. `owner` - This would store the address of the owner of a particular seed. It is of the `address` type
2. `seedName` - This stores the name of the seed. it is of type `string`.
3. `seedImgUrl` - This stores the image URL of the seed, it is of type `string`.
4. `seedDetails` - This stores the description of the seed, it is of type `string`.
5. `seedLocation` - This stores the location of the seed, it is of type `string`.
6. `price` - This stores the price of the seed. It is a number so it's of type uint.
7. `email` - This stores the email of the seller of that seed, it is of type `string`.

Next, we would create another model called PurchasedSeedInfo. This model will be used later by a map to store seeds being purchased.

```solidity
// creating a struct to store purchased seed details.
struct PurchasedSeedInfo {
        address purchasedFrom;
        string seedName;
        string seedImgUrl;
        uint256 timeStamp;
        uint price;
        string email;
    }
```

The variables used in the above struct are:

1. `purchasedFrom` - it is of type `address`. It is used to store the address of the owner of that seed.
2. `seedName` - it is of type `string`, it stores the name of the seed.
3. `seedImgUrl` - it is of type `string`, it stores the image URL of the seed.
4. `seedDetails` - it is of type `string`, it stores the description of the seed.
5. `seedLocation` - it is of type `string`, it stores the location of the seed.
6. `price` - it is of type uint since we are storing a number. It stores the price of the seed. Its a number so its of type `uint`.
7. email - it is of type `string`, it stores the email of the seller of that seed.

After creating the two models of our seed, we would create a mapping to store the `SeedInformation` model and also the `PurchasedSeedInfo` model. 

```solidity
//mapping used to store listed seeds.
    mapping (uint => SeedInformation) internal listedSeeds;

    //mapping used to store seeds purchased.
    mapping(address => PurchasedSeedInfo[]) internal purchasedSeeds;

```

To handle multiple seeds, a mapping is needed where you can access the value of a seed through its key. Just like dictionaries in Python or objects in Javascript.

To create a mapping, you use the keyword `mapping` and assign a key type to a value type. In this case, your key would be an integer and the value would be the struct `SeedInformation` we just created.


The second mapping stores the `PurchasedSeedInfo` model which is all seeds purchased by a particular buyer. This time the mapping method uses the address of the buyer as its key to store all seeds purchased by that particular buyer in an array.

In the next section, you will define a function to add the seed to the smart contract called `listSeed`.

```solidity
// Function used to list a seed.

    function listSeed(string memory _seedName, string memory _seedImgUrl,
    string memory _seedDetails, string memory  _seedLocation, uint _price, string memory _email) public {

        require(bytes(_seedName).length > 0, "seedName cannot be empty");
        require(bytes(_seedImgUrl).length > 0, "seedImgUrl cannot be empty");
        require(bytes(_seedDetails).length > 0, "seedDetails cannot be empty");
        require(bytes(_seedLocation).length > 0, "seedLocation cannot be empty");
        require(bytes(_email).length > 0, "email cannot be empty");
        
        listedSeeds[listedSeedLength] = SeedInformation({
        owner : payable(msg.sender),
        seedName: _seedName,
        seedImgUrl: _seedImgUrl,
        seedDetails : _seedDetails,
        seedLocation: _seedLocation,
        price : _price,
        email : _email
      });
     listedSeedLength++;
}
```

The function includes parameter names and its type. We use the underscore in the name of the parameters to differentiate it from the struct value we are setting.  The function has its visibility  type set to  `public`.

Next, we use the `require` method to ensure that all fields that the user will fill when listing a seed in the front end should not be empty. 

The `require` method takes two parameters: 
  1. The condition 
  2. The error message 

Next, we associate the key `listedSeedLength` with a new `SeedInformaition` structure in the `listedSeeds` mapping.

The first field of the `struct` is the address of the `owner` who can receive payments. The `msg.sender` function returns the address of the entity that initiated the call and is capable of receiving payments. This address will be stored as the owner's address.
You also need to assign values to the other variables using the provided parameters.

After that, we are going to increment the `listedSeedLength` variable by 1 so as to avoid overwriting the previous listing of a seed. 


Up next we are going to create a function that will allow us to read a listed seed when a valid index or id of that seed is passed as a parameter.

```solidity
// Function used to fetch a lised seed by its id.
    function getListedSeedById(uint _index) public view returns (
        address,
        string memory,
        string memory,
        string memory,
        string memory,
        uint price,
        string memory

    ) {

        return (
            listedSeeds[_index].owner,
            listedSeeds[_index].seedName,
            listedSeeds[_index].seedImgUrl,
            listedSeeds[_index].seedDetails,
            listedSeeds[_index].seedLocation,
            listedSeeds[_index].price,
            listedSeeds[_index].email
        );
    }
```

This function will carry a parameter of `_index` to be able to retrieve the state of a particular seed alias. You also need to specify the variables you will return in the function. In this case, it would be a tuple corresponding to the variables declared in the struct. 
The function will return the address of the `owner`, `seedName`, `seedImgUrl`, `seedDetails`, `seedLocation`, `price` and `email address` of the owner. 


Up next, we are going to create a function called `buySeed` to enable users to purchase a seed on the smart contract. 

```solidity
    function buySeed(uint _index, address _owner, string memory _seedName, string memory _seedImgUrl,  uint _price, string memory _email) public payable  {
        require(_price > 0, "Price should be greater than 0");
        require(listedSeeds[_index].owner != msg.sender, "you are already an owner of this seed");
        require(
          IERC20Token(cUsdTokenAddress).transferFrom(
            msg.sender,
            listedSeeds[_index].owner,
            listedSeeds[_index].price
          ),
          "Transfer failed."
        );
        storePurchasedSeeds(_owner, _seedName, _seedImgUrl, _price, _email);
    }

```

The `buySeed` function, which is `public` and `payable`, takes the _index, _owner, _seedName, seedImgUrl, _price, _emaiil and their respective types as parameter.

It will have three require methods. 

1. The first `require` method checks if the price that is being passed is greater than 0. If the condition is true it will move to the next require method. If the condition is false, it will throw an error message: `"Price should be greater than 0"`.

2. The second `require` method ensures that the buyer of that seed should not be the same as the seller. If it is false, it trows an error saying "you are already an owner of this seed".

3. The third `require` method ensures that the cUSD transaction is successful. It then uses the ERC-20 token interface and the stored address to call the transferFrom method to transfer cUSD.

The first parameter is the address of the sender, accessed using the msg.sender method, the second parameter is the recipient of the transaction, which is the owner of the car at the given index, and the final parameter is the price of the seed at the given index. 

If the transaction is successful, it calls the `storePurchasedSeeds` function which will be discussed later as we proceed. else it throws an error message saying "Transfer failed"

Up next we create a function called `getPurchasedSeeds` to fetch the list of all seeds purchased by a user. 

```solidity
function getPurchasedSeeds() public view returns (PurchasedSeedInfo[] memory) {
    return purchasedSeeds[msg.sender];
}

``` 

The function has a visibility type of type `public` and also uses the `view` keyword since we only want to read the contract's state and returns an array of seeds of type `PurchasedseedInfo`.


Next, we are going to create the `storePurchasedSeeds` function which was called in the `buySeed` function. It stores the seeds purchased by a user.

```solidity
function storePurchasedSeeds(address _owner,
 string memory _seedName, string memory _seedImgUrl, uint _price, string memory _email) internal {
    purchasedSeeds[msg.sender].push(PurchasedSeedInfo({purchasedFrom : _owner,
    seedName : _seedName, price : _price, email : _email, seedImgUrl : _seedImgUrl, timeStamp : block.timestamp }));
}
``` 

The function accepts parameters such as `_owner`, `_seedName`, `_seedImgUrl`, `_price`, `_email`, and their respective types. It uses the address of the `buyer` as its key and stores the information provided in an array.


Next, we are going to create a `public` function called `getListedSeedLength` which returns an int value. The function returns the length of seeds created on the blockchain.

```solidity
function getListedSeedLength() public view returns (uint) {
        return (listedSeedLength);
    }
```

Here is the full code of our smart contract:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.3;

interface IERC20Token {
  function transfer(address, uint256) external returns (bool);
  function approve(address, uint256) external returns (bool);
  function transferFrom(address, address, uint256) external returns (bool);
  function totalSupply() external view returns (uint256);
  function balanceOf(address) external view returns (uint256);
  function allowance(address, address) external view returns (uint256);
  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed owner, address indexed spender, uint256 value);
}

import "@openzeppelin/contracts/utils/Strings.sol";

contract AgroCelo{
   // Declaring variables.
    uint internal listedSeedLength = 0;
    address internal cUsdTokenAddress = 0x874069Fa1Eb16D44d622F2e0Ca25eeA172369bC1;

    // Ceating a struct to store event details.
    struct SeedInformation {
        address  owner;
        string seedName;
        string seedImgUrl;
        string seedDetails;
        string  seedLocation;
        uint price;
        string email;
    }

    struct PurchasedSeedInfo {
        address purchasedFrom;
        string seedName;
        string seedImgUrl;
        uint256 timeStamp;
        uint price;
        string email;
    }

    //map used to store listed seeds.
    mapping (uint => SeedInformation) internal listedSeeds;

    //map used to store seeds purchased.
    mapping(address => PurchasedSeedInfo[]) internal purchasedSeeds;


    // Function used to list a seed.
    function listSeed(string memory _seedName, string memory _seedImgUrl,
    string memory _seedDetails, string memory  _seedLocation, uint _price, string memory _email) public {
        listedSeeds[listedSeedLength] = SeedInformation({
        owner : payable(msg.sender),
        seedName: _seedName,
        seedImgUrl: _seedImgUrl,
        seedDetails : _seedDetails,
        seedLocation: _seedLocation,
        price : _price,
        email : _email
      });
     listedSeedLength++;
}


// Function used to fetch a lised seed by its id.
    function getListedSeedById(uint _index) public view returns (
        address,
        string memory,
        string memory,
        string memory,
        string memory,
        uint price,
        string memory

    ) {

        return (
            listedSeeds[_index].owner,
            listedSeeds[_index].seedName,
            listedSeeds[_index].seedImgUrl,
            listedSeeds[_index].seedDetails,
            listedSeeds[_index].seedLocation,
            listedSeeds[_index].price,
            listedSeeds[_index].email
        );
    }


// function used to purchase a seed by another farmer.
function buySeed(uint _index, address _owner, string memory _seedName, string memory _seedImgUrl,  uint _price, string memory _email) public payable  {
        require(listedSeeds[_index].owner != msg.sender, "you are already an owner of this seed");
        require(
          IERC20Token(cUsdTokenAddress).transferFrom(
            msg.sender,
            listedSeeds[_index].owner,
            listedSeeds[_index].price
          ),
          "Transfer failed."
        );
        storePurchasedSeeds(_owner, _seedName, _seedImgUrl, _price, _email);
    }

// function used to fetch seeds purchased already by you.
function getPurchasedSeeds() public view returns (PurchasedSeedInfo[] memory) {
    return purchasedSeeds[msg.sender];
}


// function used to store purchase seed by a particular owner.
function storePurchasedSeeds(address _owner,
 string memory _seedName, string memory _seedImgUrl, uint _price, string memory _email) internal {
    purchasedSeeds[msg.sender].push(PurchasedSeedInfo({purchasedFrom : _owner,
    seedName : _seedName, price : _price, email : _email, seedImgUrl : _seedImgUrl, timeStamp : block.timestamp }));
}



// function used to get length of lised seeds.
    function getListedSeedLength() public view returns (uint) {
        return (listedSeedLength);
    }

}
 
```

## Deploying our smart contract:

To deploy the smart contract, we would need to take the following steps:

1. Compile the code by pressing `ctrl + s`

2. Install the [CeloExtensionWallet](https://chrome.google.com/webstore/detail/celoextensionwallet/kkilomkmpmkbdnfelcpgckmpcaemjcdh?hl=en) from the Google Chrome store.


3. Create a wallet and ensure you store your key phrase in a very safe place when creating your wallet to avoid permanently losing your funds. 


4. Get the Celo token for the Alfajores testnet [here](https://celo.org/developers/faucet) : below is a break down of how to do it.

![image](images/celo_get_token_from_faucet.gif)


5. Install the Celo plugin on your Remix IDE by clicking on the Plugin manager, activate Cel0, then you click on the Celo Icon.

6. On the sidebar, click on the connect button. Your Celo wallet should pop up asking for your approval. 

7. After connecting your wallet, click on the deploy button to deploy the contract. Ensure you take note of the deploy address because we will need it later. 


NB:Take note of the ABI as shown below:

![image](images/3.png)

`and that's it! you have just deployed your first smart contract on the Celo blockchain`.


## Developing the frontend:

Going further we will be using a boilerplate from dacade in building the front end to interact with our smart contract that is being deployed on the Celo blockchain by following the steps below: 

1. Open a command line interface in the folder or directory where you want to build the front end and run the code below:

```bash
git clone https://github.com/dacadeorg/celo-boilerplate-web-dapp
```

This will create a folder called `celo-boilerplate-web-dapp`. The folder contains the neccessary setup files and folders needed to build our front end and connect it with our smart contract. The three main folders you should watch out for is the: 
`contract` folder which contains:
- erc20.abi.json file
- marketplace.abi.json file
- marketplace.sol file

`Public` folder which contains the `index.html` file and the `src` folder which contains the `main.js` file.

2. Move to the root directory of the cloned repository on the same command line interface by running the code below:

```bash
cd celo-boilerplate-web-dapp
``` 

The code changes the directory in the command line interface to the root directory for us to install the dependencies that comes with the boilerplate.

3. install all the dependencies by running the code below:

```bash
npm install
```

Installing all dependencies might take a while. After the dependencies have been installed. 

4. Start up the server by running the code:

```bash
npm run dev
```

Your project should be running here **http://localhost:3000/** and a browser window should pop up showing "hello world".

After starting the server we need to open the celo-boilerplate-web-dapp folder which is the root folder in our IDE, in our case, the VSCode IDE.

### Editing the index.html file:

In the next step of the tutorial, you will begin building the foundation of your decentralized application (dApp) using HTML. You can get the `index.html` file at the public folder.

Next we will be defining our HTML document tags and `meta` tags.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
```

The code above declares the document type, adds an HTML tag, creates a head element, and adds meta tags.

Next, we will be importing some external stylesheets. we will use Bootstrap, a popular front-end library that allows you to create elegant responsive websites very fast. You can quickly choose from Bootstrap components like buttons or cards and customize them to your needs.

```html
  <!-- CSS -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl"
      crossorigin="anonymous"
    />

    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.4.0/font/bootstrap-icons.css"
    />
```

after importing the stylesheets we will also be needing the Bootstrap icons which can be imported using external an import with the code below:

```html
 <link rel="preconnect" href="https://fonts.gstatic.com" />
  <link
    href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap"
    rel="stylesheet"
  />
```

Up next, we would ne importing the font as our main font by using the style tag to style the font type and also setting the title of the page with the title tag. Here we set the title to `AgroCelo` but you can set it to any name you want as long as it is descriptive, and we close the head tag.

```html
<style>
    :root {
    --bs-font-sans-serif: "DM Sans", sans-serif;
    }
    </style>
    <title>AgroCelo</title>
    </head>
```

Up next we would be defining our body tag where we would create our notification bar, navigation bar, hero, modals, and forms. let's get started.

For the notification, bar we write this code:

```html
<body>
<!-- Displays notifications on the web page -->
    <div class="container">
          <div class="alert alert-warning fixed-top" role="alert">
            <span id="notification">??? Loading please wait...</span>
          </div>
    </div>
```

The div has the class `alert alert-warning` which will make the text appear in an alert box. The span element has the id `notification`, which you will use to select the div and insert the text that you want to display in our Javascript code.

Up next is the navigation bar which is used to show our app name and the amount of cUSD we currently have. The amount of cUSD we be dynamic as the main data will be retrieved from our Javascript file.

```html
 <!-- Navbar starts here -->
  <nav class="navbar bg-success " >
        <div class="container">
          <span class="navbar-brand m-0 h4 fw-bold text-white">AgroCelo</span>
          <span class="nav-link border rounded-pill bg-light">
            <span id="balance">0</span>
            cUSD
          </span>
        </div>
      </nav>
 <!-- Navbar ends here -->
``` 

The navbar has two spans one which shows the name of our app and the amount of cUSD we have. The second span has an id of "balance" which will be later needed in our Javascript file to render the actual cUSD balance a user has.

Up next we will be creating our hero where which is a background that tells the user what the app does.

```html
<!-- Hero starts here -->
 <div class="row bg-success text-white">
    <div class="col-md-6 p-5">
      <h4>#1 Seed Marketplace</h4>
      <p>
        AgroCelo is a celo blockchain agro project which enables farmers to
        buy and sell the plant seed on the celo blockchain.
        <p>
      <button class="btn btn-success shadow" data-bs-toggle="modal"
          data-bs-target="#addModal">List Seed</button>
    </div>

<div class="col-md-6">
    <img style="height : 300px; width : 100%;"
    src="https://www.gardeningknowhow.com/wp-content/uploads/2020/07/seed-planting.jpg" />
  </div>
</div>
 <!--Hero ends here -->
<br />
``` 

The hero is divided into two columns by wrapping it with a div that has the class of `row`. The first column contains a div with various Bootstrap styles which has an `h4` tag that explains what the app does and a button that will pop up a modal where users can fill a form and interact with our smart contract to list a seed on the Celo blockchain. The second column contains an image tag where the source of the image is gotten from the URL.

Up next we will be creating toggle buttons that will enable the users to toggle the view between the general seed listed on the market and the ones they already bought.

```html
<!-- divs showing buttons used in switching views -->
<div class="row my-4">
  <div class="col-md-4"></div>
  <div class="col-md-4">
 <nav class="nav nav-pills" id="tabs">
  <a class="nav-link active bg-success  showProducts"
  id="productTab" aria-current="page" style="cursor : pointer;">Products</a>
  <a class="nav-link showpurchased" id="purchasedTab" style="cursor : pointer;">Purchased Products</a>
</nav>
</div>
<div class="col-md-4"></div>
</div>
<!-- end of div -->
```

In our code, we used Bootstrap `nav pills` class to create two buttons one with id set to "productTab" and the other set to "purchasedTab". The reason for the ids is so that we can use or target the buttons in our Javascript code, to enable us to add and remove styles on the web page.

Up next we will be creating a div that will show all seeds listed on the smart contract:

```html
   <!-- Start of container showing listed seeds -->
        <main id="marketplace" class="row">
          <div class="d-flex mt-3 justify-content-center">
  <div class="spinner-border text-success spinner-border-sm" role="status">
    <span class="visually-hidden">Loading...</span>
  </div>
    <p class="mx-1">Fetching seeds...</p>
</div>
        </main>
        <main id="purchasedProduct" class="row d-none">
          <div class="d-flex mt-3 justify-content-center">
  <div class="spinner-border text-success spinner-border-sm" role="status">
    <span class="visually-hidden">Loading...</span>
  </div>
    <p class="mx-1">Fetching data ...</p>
</div>
        </main>
      </div>
<!-- End of container -->
``` 

The main tag contains two ids one set to "marketplace" which will be used later on in our Javascript code to target and render the seeds listed on the blockchain. While the other is set to "purchasedProduct" which we would also use later in our Javascript code to render seeds that are being purchased by the user.

Up next is the creation of our first modal. This modal is used to show the full details of a particular seed when a user clicks the `view More Details` button of a listed seed. The `modal-body` has an id of `modalHeader` the id is what we will use to render the template that will be coming from our Javascript file. Below is the code:

```html
<!-- start of modal that shows seed details -->
<div
      class="modal fade"
      id="addModal1"
      tabindex="-1"
      aria-labelledby="newProductModalLabel"
      aria-hidden="true"
      data-bs-backdrop="static" data-bs-keyboard="false"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="newProductModalLabel">Details </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body" id="modalHeader">
          </div>
          </div>
          </div>
          </div>
<!-- end of modal -->
```

Up next is another modal that will be use to add or list a seed to the smart contract.

```html
<!-- start of modal to list a seed -->
    <div
      class="modal fade"
      id="addModal"
      tabindex="-1"
      aria-labelledby="newProductModalLabel1"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="newProductModalLabel1">New </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-row">
                <div class="col">
                  <input
                    type="text"
                    id="seedName"
                    class="form-control mb-2"
                    placeholder="seed name"
                  />
                </div>
                <div class="col">
                  <input
                    type="text"
                    id="seedImgUrl"
                    class="form-control mb-2"
                    placeholder="seed image url"
                  />
                </div>
                <div class="col">
                  <input
                    type="textarea"
                    id="seedDetails"
                    class="form-control mb-2"
                    placeholder="seed details"
                  />
                </div>
                <div class="col">
                  <input
                    type="text"
                    id="seedLocation"
                    class="form-control mb-2"
                    placeholder="seed location"
                  />
                </div>
                <div class="col">
                  <input
                    type="text"
                    id="newPrice"
                    class="form-control mb-2"
                    placeholder="price"
                  />
                </div>
                <div class="col">
                  <input
                    type="email"
                    id="email"
                    class="form-control mb-2"
                    placeholder="email address"
                  />
                </div>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-light border"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            <button
              type="button"
              class="btn btn-success"
              data-bs-dismiss="modal"
              id="listSeedBtn"
            >
              List Seed
            </button>
          </div>
        </div>
      </div>
    </div>
<!-- end of modal -->

```

Inside the modals are HTML forms, a `close` and a `submit` buttons. The form will be used to fill in the necessary information needed to list a seed in our smart contract, while the submit button will interact with our smart contract to submit the details of the form. The close button closes the modal.

Finally, we are going to add the Bootstrap JS library and a library called `blockies`, which we are going to use to visualize blockchain addresses. Then we close the body and HTML tag.

```html
<script
    src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-b5kHyXgcpbZJO/tY9Ul7kGkf1S0CWuKcCD38l8YkeH8z8QjE0GmW1gYU5S9FOnJ0"
    crossorigin="anonymous"
    ></script>
    <script src="https://unpkg.com/ethereum-blockies@0.1.1/blockies.min.js"></script>
  </body>
</html>
```

Here is the full code for the HTML part of our tutorial:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />

    <!-- CSS -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl"
      crossorigin="anonymous"
    />

    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.4.0/font/bootstrap-icons.css"
    />

    <link rel="preconnect" href="https://fonts.gstatic.com" />
    
    <link
      href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap"
      rel="stylesheet"
    />
    
    <style>

      :root {
        --bs-font-sans-serif: "DM Sans", sans-serif;
        --bs-gray: #6c757d;
      }


    </style>

    <title>AgroCelo</title>
  </head>

  <body>

    <!-- Displays notifications on the web page -->
    <div class="container">
          <div class="alert alert-warning fixed-top" role="alert">
            <span id="notification">??? Loading please wait...</span>
          </div>
    </div>


   <!-- Navbar starts here -->
  <nav class="navbar bg-success " >
        <div class="container">
          <span class="navbar-brand m-0 h4 fw-bold text-white">AgroCelo</span>
          <span class="nav-link border rounded-pill bg-light">
            <span id="balance">0</span>
            cUSD
          </span>
        </div>
      </nav>
 <!-- Navbar ends here -->



 <!-- Hero starts here -->
 <div class="row bg-success text-white">
    <div class="col-md-6 p-5">
      <h4>#1 Seed Marketplace</h4>
      <p>
        AgroCelo is a celo blockchain agro project which enables farmers to
        buy and sell the plant seed on the celo blockchain.
        <p>
      <button class="btn btn-success shadow" data-bs-toggle="modal"
          data-bs-target="#addModal">List Seed</button>
    </div>

<div class="col-md-6">
    <img style="height : 300px; width : 100%;"
    src="https://www.gardeningknowhow.com/wp-content/uploads/2020/07/seed-planting.jpg" />
  </div>
</div>
 <!--Hero ends here -->


<br />

<!-- divs showing buttons used in switching views -->
<div class="row my-4">
  <div class="col-md-4"></div>

  <div class="col-md-4">
 <nav class="nav nav-pills" id="tabs">
  <a class="nav-link active bg-success  showProducts"
  id="productTab" aria-current="page" style="cursor : pointer;">Products</a>
  <a class="nav-link showpurchased" id="purchasedTab" style="cursor : pointer;">Purchased Products</a>
</nav>
</div>
<div class="col-md-4"></div>
</div>
<!-- end of div -->

<br />


 <!-- Start of container showing listed seeds -->
        <main id="marketplace" class="row">
          <div class="d-flex mt-3 justify-content-center">
  <div class="spinner-border text-success spinner-border-sm" role="status">
    <span class="visually-hidden">Loading...</span>
  </div>
    <p class="mx-1">Fetching seeds...</p>
</div>
        </main>

        <main id="purchasedProduct" class="row d-none">
          <div class="d-flex mt-3 justify-content-center">
  <div class="spinner-border text-success spinner-border-sm" role="status">
    <span class="visually-hidden">Loading...</span>
  </div>
    <p class="mx-1">Fetching data ...</p>
</div>
        </main>


      </div>
<!-- End of container -->


<!-- start of modal that shows seed details -->
<div
      class="modal fade"
      id="addModal1"
      tabindex="-1"
      aria-labelledby="newProductModalLabel"
      aria-hidden="true"
      data-bs-backdrop="static" data-bs-keyboard="false"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="newProductModalLabel">Details </h5>

            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body" id="modalHeader">
          </div>
          </div>
          </div>
          </div>
<!-- end of modal -->


<!-- start of modal to list a seed -->
    <div
      class="modal fade"
      id="addModal"
      tabindex="-1"
      aria-labelledby="newProductModalLabel1"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="newProductModalLabel1">New </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-row">
                <div class="col">
                  <input
                    type="text"
                    id="seedName"
                    class="form-control mb-2"
                    placeholder="seed name"
                  />
                </div>

                <div class="col">
                  <input
                    type="text"
                    id="seedImgUrl"
                    class="form-control mb-2"
                    placeholder="seed image url"
                  />
                </div>

                <div class="col">
                  <input
                    type="textarea"
                    id="seedDetails"
                    class="form-control mb-2"
                    placeholder="seed details"
                  />
                </div>



                <div class="col">
                  <input
                    type="text"
                    id="seedLocation"
                    class="form-control mb-2"
                    placeholder="seed location"
                  />
                </div>

                <div class="col">
                  <input
                    type="text"
                    id="newPrice"
                    class="form-control mb-2"
                    placeholder="price"
                  />
                </div>


                <div class="col">
                  <input
                    type="email"
                    id="email"
                    class="form-control mb-2"
                    placeholder="email address"
                  />
                </div>

              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-light border"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            <button
              type="button"
              class="btn btn-success"
              data-bs-dismiss="modal"
              id="listSeedBtn"
            >
              List Seed
            </button>
          </div>
        </div>
      </div>
    </div>
<!-- end of modal -->
    <script
    src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-b5kHyXgcpbZJO/tY9Ul7kGkf1S0CWuKcCD38l8YkeH8z8QjE0GmW1gYU5S9FOnJ0"
    crossorigin="anonymous"
    ></script>
    <script src="https://unpkg.com/ethereum-blockies@0.1.1/blockies.min.js"></script>
  </body>
</html>
```

### Editing the `marketplace.sol` and the `marketplace.abi.json` file:

Before going into the `main.js` file,  we need to be able to read and write from our smart contract in our Javascript file by getting the bytecode, and to be able to do that we need to go to `Remix IDE` where we already wrote our smart contract to compile it and deploy on the Celo Alfajores network.

You can get the `marketplace.sol` and the `marketplace.abi.json` file in the contract folder of our boilerplate. 

To interact with our smart contract that is deployed in bytecode, we need an interface, the ABI (Application Binary Interface), so that the `contractKit` in our `main.js` file can understand the bytecode. The ABI allows you to call functions and read data [Learn more about the ABI](https://docs.soliditylang.org/en/develop/abi-spec.html).

Follow the steps below to edit the files:

1. Compile your smart contract on the Remix IDE and ensure it has no error message.

2. Copy the smart contract code and paste it in the `marketplace.sol` file of our boilerplate.

3. Copy the ABI code from the Remix IDE and paste it into the `marketplace.abi.json` file.

![image](images/3.png)

### Editing the `main.js` file:

The `main.js file` is the file that enables us to interact with our smart contract from our front end. You can get the `main.js` file at the `src` folder. At the beginning of the `main.js` file, the necessary libraries and files needs to be imported.

```js
import Web3 from "web3"
import { newKitFromWeb3 } from "@celo/contractkit"
import BigNumber from "bignumber.js"
import marketplaceAbi from "../contract/marketplace.abi.json"
import erc20Abi from "../contract/erc20.abi.json"

const ERC20_DECIMALS = 18
const MPContractAddress = "0x93C2eFb0Bc6d5f09D37af265B9B78c95e7dC69E4" // deployed smart contract address
const cUSDContractAddress = "0x874069Fa1Eb16D44d622F2e0Ca25eeA172369bC1" //Erc20 contract address
```

In the above code we imported Web from web3. web3.js is a  popular collection of libraries also used for ethereum, that allows you to get access to a web3 object and interact with node's JSON RPC API .

Then we import `newKitFromWeb3` from the "@celo/contractkit". The contractkit library enables us  to interact with the celo blockchain.

In order to interact with our smart contract that is deployed in bytecode, you need an interface, the ABI (Application Binary Interface), so that the contractKit can understand the bytecode. The ABI allows you to call functions and read data [Learn more about the ABI](https://docs.soliditylang.org/en/develop/abi-spec.html).

To interact with our smart contract on our front end we need the ABI of our smart contract which we previously copied in the `marketplace.abi.json` file.

After following the steps above, we import the ABI by typing `import marketplaceAbi from "../contract/marketplace.abi.json"`.

Celo's operations often deal with numbers that are too large for Javascript to handle. To handle these numbers, we will use the package bignumber.js.

The `erc20Abi` enables us to interact with the cUSD token which we will use to make payments

Next, we will create a variable called `ERC20_DECIMALS` and set its value to 18. The cUSD uses 18 decimal places.

The `MPContractAddress` is the address that is shown when we deploy our smart contract on the Celo network. On Remix, after the deployment of your contract, you will find the address to that contract which you need to interact with the functionality in your smart contract.

Then we will create a variable called `cUSDContractAddress` for the cUSD contract address which is gotten by default. All you have to do is to paste the address which is: `0x874069Fa1Eb16D44d622F2e0Ca25eeA172369bC1` 

Going further we will create three more global variables below:

```js
let kit //contractkit
let contract // contract variable
let listedSeeds = [] // array of listed seeds
```

The `kit` variable is used to store the address of a user that is connected with his/her Celo wallet. Later on, we will take a look at how it works.

The `contract` variable stores an instance of the marketplace contract once a user connects their Celo wallet, so you can interact with it.

The `listedSeeds` array stores the seeds that will be listed on the blockchain soon.

In order to store information in the `kit` and `contract` variables we need to create an asynchronous function called `connectCeloWallet` that allows a user to connect to the Celo Blockchain and read the balance of their account. The function will perform several checks and actions to ensure that the user has the necessary tools and permissions to interact with the Celo Blockchain.

```js
//Connects the wallet gets the account and initializes the contract
const connectCeloWallet = async function () {
  //checks if wallet is avaliable and gets the account.
  if (window.celo) {
    notification("?????? Please approve this DApp to use it.")
    try {
      await window.celo.enable()
      notificationOff()

      const web3 = new Web3(window.celo)
      kit = newKitFromWeb3(web3)

      const accounts = await kit.web3.eth.getAccounts()
      kit.defaultAccount = accounts[0]

      contract = new kit.web3.eth.Contract(marketplaceAbi, MPContractAddress)
    } catch (error) {
      notification(`?????? ${error}.`)
    }
    notificationOff()
  }
  // if wallet is not avaliable excute enable the notification
  else {
    notification("?????? Please install the CeloExtensionWallet.")
  }
}
```

The first step is to check if the user has the CeloExtensionWallet installed by checking if the "window.celo" object exists. If it does not exist, the function will use the console to inform the user that they need to install the wallet.

If the "window.celo" object does exist, a notification will be sent to the user in the console to approve this DApp and try the `window.celo.enable()` function. This will open a pop-up dialogue in the UI that asks for the user's permission to connect the dApp to the CeloExtensionWallet.

If an error is caught during this process, the user would be informed that they must approve the dialogue to use the dApp.

After the user approves the dApp, create a web3 object using the `window.celo` object as the provider. This web3 object can then be used to create a new kit instance, which will be saved to the kit state. This kit instance will have the functionality to interact with the Celo Blockchain.

You would then access the user's account by utilizing the web3 object and kit instance that have been created. 

After creating the new kit instance, use the method `kit.web3.eth.getAccounts()` to get an array of the connected user's addresses. Use the first address from this array and set it as the default user address by storing it in`kit.defaultAccount`. This will allow the address to be used globally in the dApp.

Next, we create a function called `approve` which will be used later to enable us to get the user approval before making a transaction on the blockchain. It takes the `_price` of the seed as a parameter. This function will be used later.

```js
async function approve(_price) {
  const cUSDContract = new kit.web3.eth.Contract(erc20Abi, cUSDContractAddress)

  const result = await cUSDContract.methods
    .approve(MPContractAddress, _price)
    .send({ from: kit.defaultAccount })
  return result
}
```

In the `approve` function, we create a cUSD contract instance with the ABI and the contract address, cUSDContract, which enable us to call the cUSD contract method `approve`. You need to specify both the contract address that will be allowed to make transactions and the amount that it will be allowed to spend, i.e., the price of the seed.

Again, you also need to specify the sender of the transaction. In this case, it's the address stored in `kit.defaultAccount`. It returns the result for error handling if there is one.

Up next we create an asynchronous function called getBalance which we use in getting the cUSD balance of the user and displaying it on the navbar we created in our HTML file.

```js
  // gets the balance of the connected account
const getBalance = async function () {
  const totalBalance = await kit.getTotalBalance(kit.defaultAccount)
  // gets the balance in cUSD
  const cUSDBalance = totalBalance.cUSD.shiftedBy(-ERC20_DECIMALS).toFixed(2)
  document.querySelector("#balance").textContent = cUSDBalance
}
  };
```

We start by calling the `kit.getTotalBalance(address)` method, passing in the user's address. This method returns the user's balance in the form of an object that contains the amounts of CELO and cUSD tokens. The returned balance is then stored in the `totalBalance` variable.

The next step is to extract the CELO and cUSD balance from the "totalBalance" object by using the `.CELO` and `.cUSD` properties respectively. Then it's shifted the value by -ERC20_DECIMALS which is a way to represent the balance in terms of smaller units in our case 18 decimal places, and then it's converting the value to fixed 2 decimal points. These values are stored in the `celoBalance` and `USDBalance` variables.

Up next, create a function called `getListesSeeds` that retrieves the seeds information stored on the blockchain and stores it in the global array called `listedSeeds` we create above.

```js
 // an async function used to get the listed seeds.
const getListedSeeds = async function() {
// a smartcontract call used to get listed seed length.
  const listedSeedLength = await contract.methods.getListedSeedLength().call()

  //initializing listSeed array
  const _listedSeeds = []

  //  function that loops through all the listSeeds.
  for (let i = 0; i < listedSeedLength; i++) {
    let seed = new Promise(async (resolve, reject) => {

  // a smartcontract call used to get listed seed by id.
      let p = await contract.methods.getListedSeedById(i).call()
      resolve({
        index: i,
        owner: p[0],
        seedName: p[1],
        seedImgUrl: p[2],
        seedDetails: p[3],
        seedLocation: p[4],
        price: new BigNumber(p[5]),
        email : p[6]
      })
    })

    // push the items on the _listedSeed array
    _listedSeeds.push(seed)
  }

  // resolves all promise
  listedSeeds = await Promise.all(_listedSeeds)
  renderProductTemplate()
}
```

Before retrieving the listed seeds, we need to know the number of seeds listed already to enable us to iterate over them. To do that we start by calling the `contract.methods.getSeedLength().call()` method, which returns the number of seeds that are stored in the smart contract. This value is stored in the `listedSeedLength` variable.

Next, we will create an empty array called `_listedSeeds` that will be used to store the listed seed objects. Next, we loop through each seed, and for each seed, we create a promise by calling the `contract.methods.getListedSeedById(i).call()` to get the listed seed data. We then resolve the promise with the seed's data and then push the seed object to the `_listedSeeds` array. Be aware that the `price` needs to be a `bigNumber` object so we can use it later to make accurate payments

After the loop is finished, we wait for all promises in the `listedSeeds` array to be resolved by calling `await Promise.all(_listedSeeds)`, this will make sure that all the listed seeds have been retrieved before calling the `renderProductTemplate` function which will be created later. We use the `renderProductTemplate` function which we will create next to show the listedSeeds  on the web page.

Next, we will create the `renderProductTemplate` function. In the `renderProductTemplate` function, we get the id of the element in which we want to render the seeds in our case the `marketplace`. Firstly we need to clear the DOM to prevent duplication of existing seeds that have already been rendered. Next, we check if there is a seed in the listSeeds array if it is true, we use the foreach loop to create a new div and set the productTemplate which we will discuss next to it and then we append the new div to the `marketplace`. 

```js
// function used to render a html template of listed seeds.
function renderProductTemplate() {
  document.getElementById("marketplace").innerHTML = ""
  if (listedSeeds) {
  listedSeeds.forEach((seed) => {
    const newDiv = document.createElement("div")
    newDiv.className = "col-md-3"
    newDiv.innerHTML = productTemplate(seed)
    document.getElementById("marketplace").appendChild(newDiv)
  })}
}
```

Next, let's create the `productTemplate` function. The `productTemplate` function returns an HTML element  and accepts a parameter called `seed`.

```js
// function that create a html template of listed seeds
function productTemplate(seed) {
  return `
 <div class="card mb-4">
      <img class="card-img-top" src="${seed.seedImgUrl}" alt="..." style="height : 150px;">
  <div class="card-body text-left p-3 position-relative">
        <div class="translate-middle-y position-absolute top-0 end-0"  id="${seed.index}">
        ${identiconTemplate(seed.owner)}
        </div>
        <p class="card-title  fw-bold mt-2 text-uppercase">${seed.seedName}</p>
        <p class="mt-2 text-left fs-6">
           ${new BigNumber(seed.price).shiftedBy(-ERC20_DECIMALS).toFixed(2)} cUSD
        </p>
        <p class="card-text mt-4">
           <div> <a class="btn btn-md btn-success view"
           id="${seed.index}" style="width:100%;">View More Details</a></div>
          </div>
    </div>
    `
}
```

Inside it should have a card that contains an `image tag`, and an `identiconTemplate` function which collects a user address and displays it as an icon to differentiate different users and three paragraphs. They should all receive an object variable named `seed.seedImgUrl`, `seed.owner`, `seed.price`, and `seed.seedName`. The `seed.price` has to be converted into a `bigNumber`.

Up next we would create the identiconTemplate function. It recieves a parameter of _address. 

```js
// function  that creates an icon using the contract address of the owner
function identiconTemplate(_address) {
  const icon = blockies
    .create({
      seed: _address,
      size: 5,
      scale: 10,
    })
    .toDataURL()

  return `
  <div class="rounded-circle overflow-hidden d-inline-block border border-white border-2 shadow-sm m-0">
    <a href="https://alfajores-blockscout.celo-testnet.org/address/${_address}/transactions"
        target="_blank">
        <img src="${icon}" width="40" alt="${_address}">
    </a>
  </div>
  `
}
```

The function returns a div that has an image tag with its `src` set to the variable icon declared at the top inside the function.

Next, we create two functions called `notification()` and `notificationOff()`. The `notification()` function displays the alert element with the text in the parameter and the `notificationOff()` function stops showing the alert element. They will be used when receiving and resolving promises using the try and catch block in our code to display error or success messages.

```js
// function to create a notification bar
function notification(_text) {
  document.querySelector(".alert").style.display = "block"
  document.querySelector("#notification").textContent = _text
}


// function to turn off notification bar based on some conditions
function notificationOff() {
  document.querySelector(".alert").style.display = "none"
}
```
Next, we would be initializing some functions each time when the window loads by using the function `window.addEventListener()`. It receives two parameters (1) load and (2) an async function that calls several functions.

```js
// initialization of functions when the window is loaded.
window.addEventListener("load", async () => {
  notification("??? Loading...")
  await connectCeloWallet()
  await getBalance()
  await getListedSeeds()
  notificationOff()
  });
```

The `async` function calls the notification function with a loading message, displays the user's balance, renders all seeds so the user can see them, and disables the notification div again once the dApp is loaded.

Next would be collecting the values of the forms we created in our modal. Firstly we get the id of our modal button, then we add an event listener to check when the button is being clicked. Next, we store the values in an array called params and we use the try block to call the `contract.methods.listedSeed()` with the params passed inside it, then we use the catch block to handle any errors that might occur. If all is successful, we call the notification, notificationOff, and getListedSeeds functions. Below is the code:

```js
document
  .querySelector("#listSeedBtn")
  .addEventListener("click", async (e) => {

// collecting form parameters
    const params = [
      document.getElementById("seedName").value,
      document.getElementById("seedImgUrl").value,
      document.getElementById("seedDetails").value,
      document.getElementById("seedLocation").value,
      new BigNumber(document.getElementById("newPrice").value)
      .shiftedBy(ERC20_DECIMALS)
      .toString(),
      document.getElementById("email").value
    ]
    notification(`??? Listing your seed on the celo blockchain...`)
    try {
      const result = await contract.methods
        .listSeed(...params)
        .send({ from: kit.defaultAccount })
    } catch (error) {
      notification(`?????? ${error}.`)
    }
    notification(`???? Listing successful`)
    notificationOff()
    getListedSeeds()
  })
```

You start by creating a new `BigNumber` instance with the `_price` argument, and then shifting it by the value stored in the `ERC20_DECIMALS` variable to convert it to `wei`.

Next, we would create a querySelector that targets an id of `marketplace` and checks for click events. This query selector enables us to display the details of a seed in a modal when the user clicks on  the view more details button:

```js
document.querySelector("#marketplace").addEventListener("click", async (e) => {
    if(e.target.className.includes("view")){
      const _id = e.target.id;
      let listedSeed;

      try {
          listedSeed = await contract.methods.getListedSeedById(_id).call();
          let myModal = new bootstrap.Modal(document.getElementById('addModal1'), {backdrop: 'static', keyboard: false});
          myModal.show();

``` 

We first of all check if the element clicked has a class name of `view` if it is true, we declare a variable called `_id` to get the id of the button click. The value of the _id acquired is what will be used to fetch the details of a seed by calling the `contract.methods.getListedSeedById(_id).call();` on the try block and storing the value on the local variable called `listedSeed` declared in the function.

On the try block also create a variable called `myModal` which is used to control the modal with the id `addModal1` in our HTML file. Through it, we got the `myModal.show()` which is used to pop up the modal when the promise is received.

Further more, let's create the details of the modal

```js
document.getElementById("modalHeader").innerHTML = `
<div class="card">
  <img class="card-img-top"
  src="${listedSeed[2]}"
  alt="image pic" style={{width: "100%", objectFit: "cover"}} />
  <div class="card-body">
    <p class="card-title fs-6 fw-bold mt-2 text-uppercase">${listedSeed[1]}</p>
    <p  style="font-size : 12px;">
      <span style="display : block;" class="text-uppercase fw-bold">Seed Description: </span>
      <span class="">${listedSeed[3]}</span>
    </p>


        <p class="card-text mt-2" style="font-size : 12px;">
          <span style="display : block;" class="text-uppercase fw-bold">Location: </span>
          <span >${listedSeed[4]}</span>
        </p>

        <p class="card-text mt-2" style="font-size : 12px;">
          <span style="display : block;" class="text-uppercase fw-bold">Email: </span>
          <span >${listedSeed[6]}</span>
        </p>

        <div class="d-grid gap-2">
          <a class="btn btn-lg text-white bg-success buyBtn fs-6 p-3"
          id=${_id}
          >
            Buy for ${new BigNumber(listedSeed[5]).shiftedBy(-ERC20_DECIMALS).toFixed(2)} cUSD
          </a>
        </div>
  </div>
</div>

  `}
```

Inside the modal, we will create a div with the class `card` and insert the image tag with some paragraphs. the image tag contains the image of the seed while the paragraph contains the details of the seed according to their order when fetched from the smart contract. Inside the modal is a buy button that will enable us to purchase the seed. We will implement the functionality later.

Still in our query selector is the catch block that handles any error that may occur when interacting with our smart contract.

```js
 catch (error) {
      notification(`?????? ${error}.`)
    }
    notificationOff()
  }
})
```

After the catch block might have handle the error by displaying it as a notification on the page, we remove the notification for other elements to be visible.

Next, we would be creating the buy functionalilty which will enable us to buy seeds from a farmer.

```js// implements the buy functionalities on the modal
document.querySelector("#addModal1").addEventListener("click", async (e) => {
    if (e.target.className.includes("buyBtn")) {

      // declaring variables for the smartcontract parameters
      const index = e.target.id
      var _price =  new BigNumber(listedSeeds[index].price)
      var _seedName = listedSeeds[index].seedName
      var _seedImgUrl = listedSeeds[index].seedImgUrl
      var _email = listedSeeds[index].email
      var _owner = listedSeeds[index].owner

      notification("??? Waiting for payment approval...")


      try {
        await approve(listedSeeds[index].price)
      } catch (error) {
        notification(`?????? ${error}.`)
      }

```

We start by using a query selector to target the id `addModal1` which is the id of the modal that shows the details of a seed. In that modal is a buy button with a class name of "buyBtn". We use the if condition to ensure that the element that we clicked on should have a class name of `buyBtn` before executing the next line of instruction. 

In the next line of instruction we are going to create 6 variables. The first variable stores the id which is a numeric value of the targeted button,  while the rest of the variables gets the details of that seed in the global array according to the id passed.

The next line pop  a notification to tell the user that their request is being processed after which we are going to use a try block to approve the price of the seed we intend to buy by calling the `approve` method and passing the price of the seed as its parameter.


After payment is being approved we are going to display another notification to let the user know that we are waiting for their confirmation. Then we are going to use a try block to call the `buySeed` function of our smart contract.

```js
notification(`??? Awaiting payment for "${listedSeeds[index].seedName}"...`)
      try {
        const result = await contract.methods
          .buySeed(index, _owner, _seedName, _seedImgUrl, _price, _email)
          .send({ from: kit.defaultAccount })
        notification(`???? You successfully bought "${listedSeeds[index].seedName}".`)
        getListedSeeds()
        getBalance()
      } catch (error) {
        notification(`?????? ${error}.`)
      }

      notificationOff()
    }

  })
```

The buySeed function contains parameter such as the index of the seed, owner, seed name, price and email and then it send the transaction. If it is successful, it notifies the user and then call the `getListedSeeds` and `getBalance` function. If there is an error it will also alert the user about the error. After everything is processed we are going to ensure that the notification is turned off by calling the `notificationOff` function.

Up next we are going to create a query selector that will target the tab id and checks for the button is being clicked. This buttons is what enables us to toggle between seed listed on the blockchain and seed bought buy the user.

```js
document.querySelector("#tabs").addEventListener("click", async (e) => {
      if (e.target.className.includes("showpurchased")) {
        document.getElementById("marketplace").classList.add("d-none");
        document.getElementById("purchasedProduct").classList.remove("d-none");
        document.getElementById("productTab").classList.remove("active", "bg-success");
        document.getElementById("purchasedTab").classList.add("active", "bg-success");

        var result;

        notification(`??? Loading please wait ...`)
```

The if condition is to check if the button click has a class of `showpurchased`. If it is true it will use the `document.getElementById` to add and remove classes found in the class list of the element retrieved. This will enable us to toggle the view between the seed listed and the seed purchased.

Next we are going to create a variable called result which we will use to store the seed purchased when we call the smart contract.

The `notification` function gives the user the impression that something is processing and they should wait for it.

Up next we are going to create a try block that will fetch the seed purchased by that user.

```js
try {
           result = await contract.methods.getPurchasedSeeds().call();

           notificationOff()
          if (result.length) {
            document.getElementById(`purchasedProduct`).innerHTML = ``
        result.forEach((item) => {
          var timestamp= parseInt(item[3])

// converts timestamp to milliseconds.
var convertToMilliseconds = timestamp * 1000;

// create an object for it.
var date = new Date(convertToMilliseconds);
```

The try block firstly calls the `getPurchasedSeeds` from the smart contract and stores the promise in the `result` variable we previously created. We then call the `notificationOff` to turn off the notification. After that we have to check if the result list is not empty. If true we get the innerHTML of the id  `purchasedProduct` and set it to be empty because we are going to render some HTML template in it using the `forEach` loop later on. Before rendering, we need to convert each timestamp that is fetch into a format that can understood by the user. 

Up next we create a template that will render the purchased seeds

```js
//template that shows purchased seeds
                document.getElementById(`purchasedProduct`).innerHTML +=
                `
                <div class="card col-md-12  mb-4">
                <div class="card-body row">
                <div class="col-md-4">
                <img
                src="${item[2]}" alt="image pic" style="width: 100%; objectFit: cover; height :150px;" />

                <div class="translate-middle-y position-absolute bottom-25 start-2" >
                ${identiconTemplate(item[0])}
                </div>
                    </div>

                    <div class="col-md-8">
                    <p class="card-text mt-2 d-flex justify-content-between" style="font-size : 12px;">
                      <span style="display : block;" class="text-uppercase fw-bold">Seed Name: </span>
                      <span >${item[1]}</span>
                    </p>


                    <p class="card-text mt-2 d-flex justify-content-between" style="font-size : 12px;">
                      <span style="display : block;" class="text-uppercase fw-bold">Price: </span>
                      <span >${new BigNumber(item[4]).shiftedBy(-ERC20_DECIMALS).toFixed(2)} cUSD</span>
                    </p>

                    <p class="card-text mt-2 d-flex justify-content-between" style="font-size : 12px;">
                      <span style="display : block;" class="text-uppercase fw-bold">Date Purchased: </span>
                      <span >${date.getHours() + ":" + date.getMinutes() + ", "+ date.toDateString()}</span>
                    </p>

                    <p class="card-text mt-2 d-flex justify-content-between"
                    style="font-size : 12px;">
                      <span style="display : block;"
                      class="text-uppercase fw-bold">Email: </span>
                      <span >${item[5]}</span>
                    </p>
                      </div>
                    </div>
                  </div>`
                  ;
              })
      } 
```  

The template contains a card that is rendered in the `purchasedProduct` id. It contains various p tags with their necessary information, an image tag to display the image, and the identiconTemplate icon that shows the address of the user purchased from in form of an icon. the item promise is being used according to how it is arranged in the smart contract.

Next, we are going to handle when the result is empty after the promise is returned.

```js
 else{
        document.getElementById(`purchasedProduct`).innerHTML = `<p class="text-center">
        you haven't purchased any seed yet</p>`;
      };

        } catch (error) {
          notification(`?????? ${error}.`)
        }
        notificationOff()
        getListedSeeds()

      }
```
The innerHTML of the id `purchaseProduct` is replaced with the text "you haven't purchased any seed yet" and a catch block is used to handle errors and display them.

After everything has been resolved we are going to turn off the notifications and call the `getListedSeeds` function.

Next, we going to handle the toggle button that shows the listed seeds.

```js
// toggles the view on the web page
      else if (e.target.className.includes("showProducts")) {
        document.getElementById("marketplace").classList.remove("d-none");
        document.getElementById("purchasedProduct").classList.add("d-none");
        document.getElementById("productTab").classList.add("active", "bg-success");
        document.getElementById("purchasedTab").classList.remove("active", "bg-success");
      }
})
```

We use the else if to check if the button clicked contains the class name `showproducts`. if it evaluates to `true`, we are going to remove and add some styles to the web page by using the `.classList.add()` function for add and .classList.remove() to remove a class

## Deployment on Github pages

Up next we will be looking at how to deploy our dApp on Github page. Before that we need to ensure that our app is working smoothly. After that you can build your dApp in the root directory command-line interface of our `celo-boilerplate-web-dapp`  with the command.

```bash
npm run build
```

After building it successfully, you should have an HTML and JS file inside the docs folder of your project.

- Upload your project to a new GitHub repository.
- Once inside your repository, click on settings and scroll down to a section called Pages.
- Select the main branch and the docs folder as the source for your GitHub pages, and click on save.
- Create a custom domain name, and click on save. Your domain name should appear at the top of the browser.
- Click on "visit site". It might take a minute or two before the site will be ready.

## Conclusion:

Congrats ????, you were able to build and deploy your full-stack dApp using Solidity and Javascript on the Celo blockchain.

## Next step:

You can challenge yourself by adding more functionalities to the smart contract and implement them using Javascript. 

The source code of this project is hosted [here](https://github.com/SamsonAmos/AgroCelo1). You can use it as a source of reference to edit yours.

- [Live Demo](https://samsonamos.github.io/AgroCelo1/)

## About the Author:

Samson Amos is a web2 and a web3 developer who loves a coding as well as teaching others. You can reach me out on Twitter [@SamsonAmoz](https://twitter.com/SamsonAmoz)

