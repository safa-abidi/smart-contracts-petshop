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

[Tutorial Link](https://trufflesuite.com/guides/pet-shop/)

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

Now let the serious part begin :). <br/>
We are going to create a smart contract which is automated, code-based agreements on a blockchain that execute predefined actions when conditions are met, eliminating the need for intermediaries. <br/>

To create the smart contract we're going to use Solidity which is an object-oriented, high-level language for implementing smart contracts. 

[Adoption Smart Contract Code](https://github.com/safa-abidi/smart-contracts-petshop/blob/main/contracts/Adoption.sol)

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

Now we need to compile our Solidity so it can be executed by the Ethereum Virtual Machine (EVM). We simply execute this command:
```
truffle compile
```
We get this result:
![Alt text](/Images/Compile.png "Optional title")

