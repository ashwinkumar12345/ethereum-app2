# ethereum-app2

Lottery Contract
================

Goal
----
- Create a lottery contract
- The lottery contract has a prize pool and a list of people who have entered for the prize pool
- Imagine two players P1 and P2 who have sent some amount of ether to the lottery contract
- P1 and P2 will be recorded as players in the game
- The ether they send will be held in a prize pool of sorts
- After some amount of time, a third party called a manager will tell the contract to pick a winner
- The manager doesn't pick the winner, the manager only tells the contract to pick a winner
- At that point, the contract will look at its list of participants, randomly pick one of the players (say P1) and send all the money from the prize pool to that winner (P1)
- The lottery contract resets and becomes ready to accept a new list of players and repeat itself all over again

![lottery-contract](https://user-images.githubusercontent.com/4720428/50492192-0ea1b300-09cb-11e9-88e9-5ce1a84bdddd.png)
