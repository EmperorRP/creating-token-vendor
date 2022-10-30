# Creating a Crypto Token Vendor (theweekndcoin)
I created a decentralized token that can be used for transactions through a challenge from Scaffold-Eth
This is the solution for the challenge 2 of Scaffold Eth - creating a token vendor. This particular code passes all the given 6 tests and has been accepted by [speedrunethereum](https://speedrunethereum.com/)

The following links will help you find my contracts and transactions:

* YourToken.sol Contract Code - [here](https://goerli.etherscan.io/address/0xBCAeA7B15a984973Fa283dA01d43aF6Ec2cefd99#code)
* Vendor.sol Contract Code - [here](https://goerli.etherscan.io/address/0x7282c9f8f52fDa725c583E9Bf6fd0eDbB05d098b#code)
* Deployed at this link - https://royal-rock.surge.sh/

## Understanding the YourToken.sol
```solidity
pragma solidity 0.8.4;
// SPDX-License-Identifier: MIT

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract YourToken is ERC20 {
    constructor() ERC20("TheWeekndCoin", "ABEL") payable {
        _mint(msg.sender, 2000 * 10 ** 18);
        approve(address(this), 1000);
    }
}
```

We declare a new contract with the name YourToken which inherits the properties of ERC20


