# ethereum-app2

> ## Contents

**[Lottery Contract](#lotterycontract)**<br>
**[Design Considerations](#designconsiderations)**<br>
**[Basic Solidity Types](#basicsoliditytypes)**<br>
**[Starting the Lottery Contract](#startingthelotterycontract)**<br>
**[Reference Types](#referencetypes)**<br>
**[Solidity Gotcha](#soliditygotcha)**<br>
**[Entering the Lottery](#enteringthelottery)**<br>
**[Validating the Contract](#validatingcontract)**<br>
**[Using Remix Debugger](#remixdebugger)**<br>
**[Pseudo Random Number Generator](#pseudorandomnumbergenerator)**<br>
**[Selecting a Winner](#selectingawinner)**<br>
**[Sending Ether to the Winner](#sendingethertothewinner)**<br>
**[Resetting Contract State](#resettingcontractstate)**<br>
**[Requiring Managers](#requiringmanagers)**<br>
**[Function Modifier](#functionmodifier)**<br>
**[Returning Players Array](#returningplayersarray)**<br>
**[New Test Setup](#newtestsetup)**<br>

<a name="lotterycontract"></a>
> ## Lottery Contract 

- This project is to create a lottery contract
- The lottery contract has a prize pool and a list of people who have entered for the prize pool
- Imagine two players P1 and P2 who have sent some amount of ether to the lottery contract
- P1 and P2 will be recorded as players in the game
- The ether they send will be held in a prize pool of sorts
- After some amount of time, a third party called a manager will tell the contract to pick a winner
- The manager doesn't pick the winner, the manager only tells the contract to pick a winner
- At that point, the contract will look at its list of participants, randomly pick one of the players (say P1) and send all the money from the prize pool to that winner (P1)
- The lottery contract resets and becomes ready to accept a new list of players and repeats itself all over again

![lottery-contract](https://user-images.githubusercontent.com/4720428/50492192-0ea1b300-09cb-11e9-88e9-5ce1a84bdddd.png)

<a name="designconsiderations"></a>
> ## Design Considerations 

- At a basic level, you need to store two basic pieces of information in the contract:
- Variables: 
    - manager - A variable to store the address of the person who created the contract
    - players - An array of addresses of the people who have entered the contract (as soon as somebody sends ether to the contract, they are automatically entered into it)
- Functions: 
    - enter - A function that someone can send ether to enter the contract
    - pickWinner - A function that randomly picks a winner and sends them the entire amount of ether from the prize pool

<a name="basicsoliditytypes"></a>
> ## Basic Solidity Types

- Here are some of the basic variable types in Solidity:
    - string - Sequence of characters. For example, "Hi there!", "Bye there"
    - bool - Has boolean values. For example, true or false
    - int - Integer, positive or negative. No decimal. For example, -3000, 58209
    - uint - Integer, positive only. For example, 20
        - int32, uint16, etc - You can specify the number of bits to be used to specify an int or uint 
        - By default, int = int256, uint = uint256
        - You can always just use uint or int if you are not sure about the exact size of bits to use
        - You would pay for all extra bits that you would use
        - The increase in price is not very significant
    - fixed/ufixed - Decimal numbers. Usually used to store math calculations. For example, 3.14, -43.46
        - Contracts in general are way more about storing raw data and implementing basic business logic 
        - As soon as you start doing complex math, you need to pay for every operation that you perform inside a contract
    - address - Store addresses. Has methods tied to it for sending money. For example, 0x632tbjh32782..
    
<a name="startingthelotterycontract"></a>
> ## Starting the Lottery Contract

- Open your Remix code editor
- Delete any existing contract in the browser window

      pragma solidity ^0.4.17
     
      //Version of the compiler
      //Contract will be valid for newer versions of Solidity
      
      contract Lottery {
      
          address public manager;
      
          function Lottery() public {
      
              manager = msg.sender;
              
             //msg object is a global variable
             //msg describes who sent the transaction to the network and some details about the transaction
             //msg is available to any function invocation
             //msg.data - Arguments that we want to send to that function
             //msg.gas - Amount of gas you have available to run some code
             //msg.sender - Address of the person who sent a transaction or call to the contract
             //msg.value - Amount of ether (in wei) that has been sent
             
          }
      }
      
- Click the Run tab 
- Select Javascript VM as the environment
- Click Create, will create a new instance
- Click the manager variable, you will see an address
- The address will match the address of the account you have selected

<a name="referencetypes"></a>
> ## Reference Types

- Arrays is an example of a reference types:
    - fixed array - Contains a single type of element. Has unchanging length. For example, `int[3] -> [1, 2, 3]`, `bool[2] -> [true, false]`
    - dynamic array - Contains a single type of element. Can change in length over time. For example, `int[] -> [1, 2, 3, 4, 5]`
    - mapping - Collection of key value pairs. Think of Javascript objects, Ruby hashes, or Python dictionary. All keys same type and all values same type. For example, a collection of different cars, or houses `mapping(string=>string)` 
    - struct - Collection of key value pairs that can have different types. For example, a collection of a singular thing `struct Car{string make; string mode; uint value;}`
    
<a name="soliditygotcha"></a>
> ## Solidity Gotcha

- You can build a two-dimensional array in Solidity. For example, `const myArray = [[1, 2, 3], [1, 2, 3], [1, 2, 3]];`
- The gotcha is that you don't have the ability to pull a two-dimensional array from the Solidity world into the Javascript world
- Strings inside Solidity is represented as dynamic array
- You cannot transfer array of strings into Javascript

<a name="enteringthelottery"></a>
> ## Entering the Lottery

- Open your Remix editor and continue:

      pragma solidity ^0.4.17
     
      contract Lottery {
      
        address public manager;
        address[] public players;
        
        function Lottery() public {
            manager = msg.sender;
          }
        function enter() public payable {
            require (msg.value > 0.01 ether)
            //Used for validation
            //To make sure that the sender sends in atleast 0.01 ether to enter the lottery
            players.push(msg.sender);
          }
        } 
      }
      
<a name="validatingcontract"></a>
> ## Validating the Contract

- Click the `Run` tab
- In the Environment tab, select `Javascript VM`
- Click the `Create` button
- Click the `manager` variable and you will get back your account address
- Click the `enter` variable
- In an array, you can only access one variable at a time
- in the `players` variable, enter `0` and click `players`
- You will get back your own account address
- Select a different account and click `enter`
- In the `players` array, enter `1` and click `players`
- You will see the new account address 
- You need to add a require statement to make sure that the sender sends in some min ether to enter the contract

      function enter() public payable {
        require (msg.value > 0.01 ether)
        //Used for validation
        //To make sure that the sender sends in atleast 0.01 ether to enter the lottery
        players.push(msg.sender);
      }
          
 - Cancel the contract
 - Click `Create` to redeploy the contract
 - Specify 0.11 ether in the `Value` tab
 - Click the `enter` variable
 - You can select a another account and specify 0.00001 ether and click `enter`
 - You will see an error message and the transaction will not go through
 
<a name="remixdebugger"></a>
> ## Using Remix Debugger

- In the transaction log, you see two buttons, `Details` and `Debug`
- If you click `Details`, you will see some info about the transaction
- If you click `Debug`, the right-hand panel changes
- You can use this tool to step through the execution of your code, to get an understanding of how data is flowing through your contract
- The sliding bar allows you to fast forward or rewind the exection of your contract
- You can click step in arrow to step through the exection of each step 
- You can also see the current state of your contract, by clicking `Solidity State` dropdown

<a name="pseudorandomnumbergenerator"></a>
> ## Pseudo Random Number Generator

- Once a certain number of players have entered the contract, the manager should be able to call a `pickWinner` function to select a winner
- Goal of the `pickWinner` function is to randomly pick a winner and send them the prize pool
- In Solidity, you don't have a random number generator, so you need to fake the randomness
- Take the current block difficulty (time to process an actual transaction, represented as an `int`, large number), current time, and addresses of players and feed them to a SHA3 algorithm
- The SHA3 algorthm will output a really big number in hex
- Take this really big hex number and use that to pick a `random` winner
- Create a helper function to implement this random function that you can call in the `pickWinner` function

      function random() private view returns (uint) {
         uint(keccak256(block.difficulty, now, players));
         //keccak is a class of algorthims and sha3 is a partcular instance of it..equivalent 
         //block and now are global variables
      }
     
<a name="selectingawinner"></a>
> ## Selecting a Winner

- Use the modulo operator to return the remainder between the random function output and players.length 
- You will get back a `random` number between 0 and players.length
- The winner would be the player at this index in the players array

      function pickWinner() public {
         uint index = random() % players.length;
      }
 
 <a name="sendingethertothewinner"></a>
> ## Sending Ether to the Winner

- Send ether to the winner:

      function pickWinner() public {
         uint index = random() % players.length;
         players[index].transfer(this.balance);
         //players[index] returns an address
         //you can transfer function on an address
         //takes all of the money in the contract and send to that player
      }
          
- Test out the contract in Remix
- Call the pickWinner function
- Make sure the ether is transferred to one of the players entered into the contract

 <a name="resettingcontractstate"></a>
> ## Resetting Contract State

- You would want to reset the contract to run a another round of the lottery immediately after picking a winner          
- For this, just empty out the players array

![lottery-contract-2](https://user-images.githubusercontent.com/4720428/50608685-350b9680-0e82-11e9-8344-c0bd981bfb73.png)

    function pickWinner() public {
       uint index = random() % players.length;
       players[index].transfer(this.balance);
       players = new address[](0); //resetting players array
    }

 <a name="requiringmanagers"></a>
> ## Requiring Managers

- You also want to make sure only the manager can pick a winner
- This is to make sure that no player can estimate the peusdo randomness

      function pickWinner() public {
         require (msg.sender == manager);
         uint index = random() % players.length;
         players[index].transfer(this.balance);
         players = new address[](0); 
      }

 <a name="functionmodifier"></a>
> ## Function Modifier

- To make the code more readable, you can add a function modifier to the contract
- Function modifiers are used to solely to avoid repeating lines of code common to different functions

      function pickWinner() public restricted {
         uint index = random() % players.length;
         players[index].transfer(this.balance);
         players = new address[](0); 
      }

      modifier restricted() {
         require(msg.sender == manager);
         _;
      }

- You can now go into any function in the contract and add the restricted keyword if you think it's an administrative-level function that should be called only by the manager

 <a name="returningplayersarray"></a>
> ## Returning Players Array

- This function returns a list of all players entered into the contract, instead of calling each element with the index
- This will make it easier for a web application to display the addresses of all players in the contract and also the number of players at any given point of time

      function getPlayers() public view returns (address[]) {
         return players;
      }

 <a name="newtestsetup"></a>
> ## New Test Setup

- You can just duplicate the inbox tests and use as a base
- Copy the inbox directory and paste as another directory
- Rename the folder as `lottery`
- Start your Atom code editor in the lottery directory
- Inside the contracts directory, rename `Inbox.sol` to `Lottery.sol`
- Replace the code in `Lottery.sol` with the code in the Remix editor
- Also, delete the `inbox.test.js` and create a new `lottery.test.js`

