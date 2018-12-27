# ethereum-app2

> ## Contents

**[Lottery Contract](#lotterycontract)**<br>
**[Design Considerations](#designconsiderations)**<br>
**[Basic Solidity Types](#basicsoliditytypes)**<br>

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
    - manager - Address of the person who created the contract
    - players - Array of addresses of the list of people who have entered (as soon as somebody sends ether to the contract, they are automatically entered into it)
- Functions: 
    - enter - A function that someone can send ether to and enter the contract
    - pickWinner - A function that randomly pick a winner and send them the entire contents of the prize pool

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
