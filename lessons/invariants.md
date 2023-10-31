
## Invariants

- mathematical constraints that will be applicable regardless of the system state
- invariants should always be provable via induction logic, ie
- Initial state should be true and any resultant state should also be true
- Often biggest challenge is coming up with invariants
- Bad invariants are ones that are too permissive - eg. `X + 2 > X` or `2*X < X => 5 > 7`




List of invariants that are useful:

- bad invariants are t



| Invariant | Description           | Platform |
|-----------|-----------------------|----------|
| Solvency    | Sum of cash > sum of commitments | Aave, Compound etc |
| Illegal address    | Cannot register illegal address  | Celo |
| Price range    | Price is within bounds | Sushiswap |
| borrow < collateral    | Every borrowing needs to have enough collateral | Aave |
| sum(balances) ==total supply    | Sum of balances of all token holders = total supply | ERC20 |
| balance of any user <=total supply    | Balance of any individual user <= total supply | ERC20 |
| liquidity shares > 0 <=> system holding >0y    | liquidity tokens only exist if system is holding underlying tokens | vaults |



