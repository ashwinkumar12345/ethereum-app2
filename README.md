# ethereum-app2

> ## Contents

**[Lottery Contract](#lotterycontract)**<br>

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
- Variables: manager - Address of the person who created the contract
             players - Array of addresses of the list of people who have entered (as soon as somebody sends ether to the contract, they are automatically entered into it)
  Functions: enter - A function that someone can send ether to and enter the contract
             pickWinner - A function that randomly pick a winner and send them the entire contents of the prize pool
  
