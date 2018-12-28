# ethereum-app2

> ## Contents

**[Lottery Contract](#lotterycontract)**<br>
**[Design Considerations](#designconsiderations)**<br>
**[Basic Solidity Types](#basicsoliditytypes)**<br>
**[Starting the Lottery Contract](#startingthelotterycontract)**<br>
**[Reference Types](#referencetypes)**<br>
**[Solidity Gotcha](#soliditygotcha)**<br>
**[Entering the Lottery](#enteringthelottery)**<br>

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
              players.push(msg.sender);
          }
      }
      
      }
