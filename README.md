# Pet Shop Truffle

Project made by: 
- [Safa Laabidi](https://github.com/safa-abidi) 
- [Ines Achour](https://github.com/inesachour)
- [Amal Sammari](https://github.com/Amal1999)

## I-Introduction
In this Project we are going to use Truffle, Ganache and Metamask.
Let's start by introducing them.

- Truffle is a development framework for Ethereum that simplifies contract creation, testing and deployment.

- Ethereum is a blockchain for smart contracts and decentralized apps with its cryptocurrency, Ether (ETH).

- Ganache is a local blockchain simulator for Ethereum development.

- MetaMask is a browser extension for Ethereum DApps and wallet functionality.

This Project is made with this [Tutorial Link](https://trufflesuite.com/guides/pet-shop/).

## II-Implementation

### 1-Set Up

As mentioned in the tutorial, we started by making sure that we already have Node.js, npm and git in our system.

After that we install Truffle using npm :
``` 
npm install -g truffle 
```

Now we execute this command to get a basic project structure for our pet shop with the user interface :
```
 truffle unbox pet-shop 
```

### 2- Project Implementation

Now let the serious part begin :smiley:. <br/>

#### A- Smart Contract
We are going to create a smart contract which is automated, code-based agreements on a blockchain that execute predefined actions when conditions are met, eliminating the need for intermediaries. <br/>

To create the smart contract we're going to use Solidity which is an object-oriented, high-level language for implementing smart contracts. <br/>
We create a file named [Adoption.sol](https://github.com/safa-abidi/smart-contracts-petshop/blob/main/contracts/Adoption.sol) in contracts directory

Let's break the code into pieces. <br/>
Starting by the first line inside the contract :
``` 
address[16] public adopters;
```
This line defines the adopters variable as an array of Ethereum addresses. Each address of this array represents the adopter of the corresponding animal.

Let's move on to this part of code in which we implement the adopt method that gets the petId verify if it actually exists (between 0 and 15 as our array's length is 16) then assign the adopter to the corresponding pet and finally return the petId:
``` 
function adopt(uint petId) public returns (uint) {
  require(petId >= 0 && petId <= 15);

  adopters[petId] = msg.sender;

  return petId;
}
```

The last part of code in the smart contract is the getAdopters function that returns the array adopters:

``` 
function getAdopters() public view returns (address[16] memory) {
  return adopters;
}
```

#### B- Compilation
Now we need to compile our Solidity so it can be executed by the Ethereum Virtual Machine (EVM). We simply execute this command:
```
truffle compile
```
We get this result:
![Compilation Output](/Images/Compile.png "Compilation Output")

#### C- Migration
Migrations are scripts that help deploy and manage smart contracts on a blockchain, facilitating the development and updating of decentralized applications. <br/>
So we create a file named [2_deploy_contracts.js](https://github.com/safa-abidi/smart-contracts-petshop/blob/main/migrations/2_deploy_contracts.js) in migrations directory <br/>

Before we can migrate our contract Adoption to the blockchain, we need to download **Ganache**
![Ganache](/Images/Ganache.png "Ganache"). <br/>
Our initial balance is ( which we can't use in real life sadly :frowning_face: ) :
![Initial Balance](/Images/InitialBalance.png "Initial Balance")<br/>

Now we execute:
```
truffle migrate
```
![Migration Output Part 1](/Images/Migrate1.png "Migration Output Part 1")
![Migration Output Part 2](/Images/Migrate2.png "Migration Output Part 2")

#### D- Testing The Smart Contract Using Solidity

#### E- Testing The Smart Contract Using Javascript

#### F- Running The Tests

#### G- User Interface

#### H- MetaMask

#### I- Testing The Pet Shop







