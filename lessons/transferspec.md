## Transfer spec


- transfer spec is defined in a file called `transfer.spec`
- first we define methods that are environment free
- then we define a rule
- in the rule, we filter out scenarios we don't want the prover to focus on using `require`
- once done, run the `certoraRun` to get the outputs on the certora UI
- by default, prover ignores reverting paths -> in some cases, we want the prover to also consider reverting paths
- `transferReverts` rule is meant to do just that - see below
- note that we use @withrevert for prover to accept reverting paths. At the same time, we also use `lastreverted` keyword to check if last call executed on contract reverted or not
- similarly, we define a liveness test which is the opposite of the test above, which is for the prover to prove that transfer does NOT revert if amount is below sender balance
- `transferdoesnotRevert` note that we had to introduce an extra filter -> `e.msg.value ==0` because transfer is not a `payable` function and EVM will revert if it sees msg.value > 0-> in a way, this violates our proof -> to remove this, we insert `msg.value==0` condition

```
	// certora transfer spec
	// first step is to define methods 
	// define balanceOf function as environment free -> does not depend on environment
	// environment here refers to the blockchain state (msg.sender, msg.value, timestamp etc)
	methods {
		balanceOf(address) returns(uint) envfree
	}

	rule transferSpec{
		address sender; address recipient; uint amount;

		env e; // environment variable
		require e.msg.sender == sender;
		require recipient != sender; //recipient == sender breaks below invariant
		// we need to filter out that condition upfront

		mathint balance_before_sender  = balanceOf(sender);
		mathint balance_before_recipient = balanceOf(recipient);

		transfer(e, recipient, amount); // pass environment variable in this
		
		mathint balance_after_sender = balanceOf(sender);
		mathint balance_afer_recipient = balanceOf(recipient);

		asssert balance_after_sender = balance_before_sender - amount, "transfer must reduce sender balance";
		assert balance_after_recipient = balance_before_recipient + amount, "transfer must increase balance";
		
	}

	rule transferReverts{
		env e;
		address sender; address recipient; uint amount;

		require e.msg.sender == sender;
		require amount > balanceOf(sender);

		transfer@withrevert(e, recipient, amount);
		assert lastReverted, "transfer must revert if amount > sender balance";
	}
	
	rule transferdoesnotRevert{
		
		env  e;
		address sender; address recipient; uint balance;
		
		require sender == e.msg.sender;
		require amount < balanceOf(sender);
		require e.msg.value == 0; // transfer is not payable function
		// if msg.value > 0, transfer will revert
		// so this below conditiion fails -> to prevent this we say msg.value == 0
	
		transfer@withrevert(e, recipient, amount);

		assert !lastReverted, "transfer must not revert with proper amount";
	}



```
