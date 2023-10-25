## Certora Prover Intro


- Human is in the loop
- Build invariants 
- Prover will find bug that is basically a violation of invariant
- whole complexity is related to writing these invariants
- a violation can happen after many many iterations
- trhat is why a human will find it tough to find such a bug
- Simple invariant examples
	- ERC20 -> total supply == sum of all balances
	- another refers to solvency -> sum of deposits less than bank holding
	- Liquidity pools -> tokenA * tokenB == lp tokens * lp token
	- transfer from addy A -> addy B should not change the total supplys
- 

## Defining a rule
- A rule is a parametric representation of an invariant
- You can narrow the universe of parameters by introducing `require`
- parameters here refer to the inputs for a function
- Here in most cases, we need to pass `env` -> its an enviornment which hosts critical info -> msg.sender, msg.value, block.number etc
- Each rule can have its own environment - apparently we can combine environments as well (not sure about this use case)
- Once a rule is defined, we can run the rule -> certora basically publishes the outputs to the cloud
- _Need to understand what happens under the hood_


## Parameterization 

- A very powerful technique used in Certora Prover
- It can abstract all the functions that can change a given state
- For eg., if `approve` function increases allowance -> then writing a rule specific to `approve` does not take advantage of parameterization
- However, if we define a generic function `f(e, args)` that takes the environment and generic arguments, then we can expand the rule to include any functions that change allownace (these could be `transfer`, `transferFrom`, `increaseAllowance`, `decreaseAllowance`
o  


