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

### Parametric rule -> only select functions can change state
- In this case, we can define a rule where we can assert that if balanceBefore != balanceAfter, then the balance change must happen by sleect function selectors. Effectively, we are ensuring there is no backdoor to allow a balance change beyond these select function

## Inductive Logic

- Most formal verification works on mathematical induction
- If P(0) = true, if P(n)=true => P(n+1) is true, then property P is true for all natural numbers
- Not everything might be inductively proven - in some cases, our setup might not be inductive
- In such cases, we need to strengthen the property so that it can be inductively proven
- Writing invariants and thinking in inductive mode is the most important takeway
- there is a network effect to writing invariants -> most protocols will have similar invariants that can be just copy pasted 


## Formal Verification

- Verification takes the code (`.sol`) and certora spec(`.spec) to formally verify proofs
- Proofs can either be proven, failed or in unknown state
- unknown state means that the process timedout and the prover was inconclusive
- formal verification is as good as the underlying rules -> it is useful with good rules
- when FV reports a bug, check it
- When FV does not report a bug, try to change the spec or even, mutate the program to make sure that FV catches is (ie. voluntarily introduce a bug and check if its caught)
- Vacuous rules are like tautologies - they always hold and provide no useful insights
- When writing rules, you should be careful not to write rules that are always true -> this gives a false sense of security that the code is correct and even more dangerous than having no rules
- Hardest problem in FV is writing a spec -> a spec that is strong enough to find bugs
- FV is great if code is upgraded regularly 

 


  


