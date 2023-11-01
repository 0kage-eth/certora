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

---


### ERC 721

_Valid Properties_

1. `user != address(0)` and `owner(id)`== user <=> `balanceOf(user) >= 1`
2. After a successful call to `transferFrom(from, to, tokenId)`, following always holds a. `owner(tokenId)==to` and `balanceOf(to)>=1`


### ERC721 Enumerable

_Valid Properties_

1. `tokenByIndex(i)==j => i < totalSupply`
2. `u != o && tokenOfOwnerByIndex(o,i)==j => ownerOf(j) != u`
