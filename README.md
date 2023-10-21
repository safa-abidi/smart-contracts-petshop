# Pet Shop Truffle

Project made by: 
- [Safa Laabidi](https://github.com/safa-abidi) 
- [Ines Achour](https://github.com/inesachour)
- [Amal Sammari](https://github.com/Amal1999)
<br/>
<br/>

The project contains these sections:<br/>
[I- Introduction](https://github.com/safa-abidi/smart-contracts-petshop#i-introduction)<br/>
[II- Project Implementation](https://github.com/safa-abidi/smart-contracts-petshop#ii-implementation)<br/>
&ensp;[1- Set Up](https://github.com/safa-abidi/smart-contracts-petshop#1-set-up)<br/>
&ensp;[2- Project Implementation](https://github.com/safa-abidi/smart-contracts-petshop#2--project-implementation)<br/>
&ensp;&ensp;[A- Smart Contract](https://github.com/safa-abidi/smart-contracts-petshop#a--smart-contract)<br/>
&ensp;&ensp;[B- Compilation](https://github.com/safa-abidi/smart-contracts-petshop#b--compilation)<br/>
&ensp;&ensp;[C- Migration](https://github.com/safa-abidi/smart-contracts-petshop#c--migration)<br/>
&ensp;&ensp;[D- Testing The Smart Contract Using Solidity](https://github.com/safa-abidi/smart-contracts-petshop#d--testing-the-smart-contract-using-solidity)<br/>
&ensp;&ensp;[E- Running The Tests](https://github.com/safa-abidi/smart-contracts-petshop#e--running-the-tests)<br/>
&ensp;&ensp;[F- User Interface](https://github.com/safa-abidi/smart-contracts-petshop#f--user-interface)<br/>
&ensp;&ensp;[G- MetaMask](https://github.com/safa-abidi/smart-contracts-petshop#g--metamask)<br/>
&ensp;&ensp;[H- Testing The Pet Shop](https://github.com/safa-abidi/smart-contracts-petshop#h--testing-the-pet-shop)<br/>

## I-Introduction
In this Project we are going to use Truffle, Ganache and Metamask.
Let's start by introducing them.

- Truffle is a development framework for Ethereum that simplifies contract creation, testing and deployment.

- Ethereum is a blockchain for smart contracts and decentralized apps with its cryptocurrency, Ether (ETH).

- Ganache is a local blockchain simulator for Ethereum development.

- MetaMask is a browser extension for Ethereum DApps and wallet functionality.

This Project is made with this [Tutorial Link](https://trufflesuite.com/guides/pet-shop/).


## II- Implementation
### 1- Set Up

As mentioned in the tutorial, we started by making sure that we already have Node.js, npm and git in our system.

After that we install Truffle using npm :
``` 
npm install -g truffle 
```

Now we execute this command to get a basic project structure for our Pet Shop with the user interface :
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

Before we can migrate our contract Adoption to the blockchain, we need to download **Ganache**.

Those are our initial blocks in Ganache:
![Ganache](/Images/Ganache.png "Ganache") <br/>


Our initial balance is ( which we can't use in real life sadly :frowning_face: ) :

![Initial Balance](/Images/InitialBalance.png "Initial Balance")<br/>

Now we execute:
```
truffle migrate
```

![Migration Output Part 1](/Images/Migrate1.png "Migration Output Part 1")
![Migration Output Part 2](/Images/Migrate2.png "Migration Output Part 2")
![Migration Output Part 3](/Images/Migrate3.png "Migration Output Part 3")

After the migration is done, we can see that the state of the blockchain has changed. We have now lower than 100 ether :frowning_face: and more blocks:

![Ganache](/Images/Ganache2.png "Ganache")

#### D- Testing The Smart Contract Using Solidity
So we create a file named [TestAdoption.sol](https://github.com/safa-abidi/smart-contracts-petshop/blob/main/test/TestAdoption.sol) in test folder <br/>

Inside the Test we can find those 3 variables:
```
Adoption adoption = Adoption(DeployedAddresses.Adoption());
uint expectedPetId = 8;
address expectedAdopter = address(this);
```

The adoption variable is the contract to be tested.<br/>
The expectedPetId represents the Id of the pet that we're going to use for the test.<br/>
exptectedAdopter is the expected adopter for the pet.

We also have this function in which we test the adopt() function:
```
function testUserCanAdoptPet() public {
  uint returnedId = adoption.adopt(expectedPetId);

  Assert.equal(returnedId, expectedPetId, "Adoption of the expected pet should match what is returned.");
}
```

We added this code to test the retrieval of the owner of a pet:
```
function testGetAdopterAddressByPetId() public {
  address adopter = adoption.adopters(expectedPetId);

  Assert.equal(adopter, expectedAdopter, "Owner of the expected pet should be this contract");
}
```

And finally we have this function that tests the retrieval of the entire list of the pet owners:
```
function testGetAdopterAddressByPetIdInArray() public {
  // Store adopters in memory rather than contract's storage
  address[16] memory adopters = adoption.getAdopters();

  Assert.equal(adopters[expectedPetId], expectedAdopter, "Owner of the expected pet should be this contract");
}
```


#### E- Running The Tests
To run the created tests we simply execute this command:
```
truffle test
```

![Testing Output](/Images/Test.png "Testing Output")<br/>

Perfect! everything is working perfectly :wink:. 


#### F- User Interface
To create the UI of our Pet Shop we need to add some code in [app.js](https://github.com/safa-abidi/smart-contracts-petshop/blob/main/src/js/app.js) file in src/js/ directory


#### G- MetaMask
Now it's time to install the MetaMask Plugin in our Browser.
After that we create a Wallet.
Then we click on the **Ethereum Mainnet** button:

![Ethereum Mainnet](/Images/CustomRPC1.png "Ethereum Mainnet")<br/>


We click on **Add Network**:

![Add Network](/Images/CustomRPC2.png "Add Network")<br/>


And now on **Add new network manually** (bottom of the screen):

![Add New Network Manually](/Images/CustomRPC3.png "Add New Network Manually")<br/>


We fill the fields as below (the New RPC URL field correspond to the RPC Server Url found in Ganache) <br/>

![Filling The Fields](/Images/CustomRPC4.png "Filling The Fields")<br/>


And finally we create a new account and that's by clicking on our account name and then on **Import Account**. We need to type the private key found in the first shown block in Ganache (by clicking on the key icon) <br/>

![New Account](/Images/NewAccount.png "New Account")<br/>


And it worked like a charm! :clap:. <br/>

![MetaMask Connected](/Images/MetaMaskConnected.png "MetaMask Connected")<br/>


#### H- Testing The Pet Shop
Now comes the exciting part where we get to choose our lovely pet :heart_eyes:.
But before that we need to start the local web server by executing:
```
npm run dev
```
We get then a MetaMask pop-up appears to request our approval to allow the Pet Shop to connect to our MetaMask wallet.<br/>

This the UI we get then: <br/>

![Pet Shop](/Images/website.png "Pet Shop")<br/>

Lovely isn't it ? :wink:. <br/>


We choose a cute pet to adopt and click on the adopt button. This confirmation popup from MetaMask will appear: <br/>

![Transaction Confirmation](/Images/AdoptConfirm.png "Transaction Confirmation")<br/>


How could we reject such a cute offer? So we obviously clicked on confirm. <br/>
We can now see that the transaction is pending. <br/>

![Transaction Pending](/Images/AdoptPending.png "Transaction Pending")<br/>


After waiting a bit the transaction was confirmed and we can see that we have a lower amount of ether. (Of course we couldn't resist the cute pets and adopted another one :wink:) <br/>

![Transaction Confirmed](/Images/AdoptConfirmed2.png "Transaction Confirmed")<br/>


We can also see that in Ganache the state of the blockchain has changed:

![Ganache](/Images/Ganache3.png "Ganache")<br/>





