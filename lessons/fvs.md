## Formal verifications


### ERC20

_Valid Properties_

1. For all users,  balanceOf(user) can only change on mint(), burn(), transfer(), transferFrom()
2. totalSupply() is the sum of balanceOf() over all users 
3. For all users: balanceOf(user) <= totalSupply()
4. When using transfer or transferFrom, `balance_before[from] + balance_before[to] == balance_after[from] + balance_after[to]`
5. When using transfer or transferFrom, `totalSupply before == totalSupply after`


_Invalid Properties_

1. For all user: balanceOf(user) can only change on operation performed when msg.sender==user or when allowance(user, msg.sender) is not zero

_This fails because `mint` function can be called by the token owner to increase balance of any user. In this case both the above conditions are not satisfied and yet the balance of the user increases_


