# Creating a Crypto Token Vendor (theweekndcoin)
I created a decentralized token that can be used for transactions through a challenge from Scaffold-Eth
This is the solution for the challenge 2 of Scaffold Eth - creating a token vendor. This particular code passes all the given 6 tests and has been accepted by [speedrunethereum.](https://speedrunethereum.com/)
> _Please try not to copy this code, I suggest you work on your own solution and if necessary, take inspiration from the code that I've written._

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

We declare a new contract with the name YourToken which inherits the properties of ERC20. 
ERC20 is a Ethereum standard which can help generate tokens that are fungible. [More info here](https://ethereum.org/en/developers/docs/standards/tokens/erc-20/)

You can check out the implementation [here](https://docs.openzeppelin.com/contracts/4.x/erc20)

The constructor can take in the name (in our case, "TheWeekndCoin") and the denoting characters (in our case, "ABEL"). Using the _mint_ function, I input 2 pieces of information - the address of the contract and the total initial supply of the coin.

Next line shows the approve function which we will come across later.

##Understanding Vendor.sol for buyTokens() function
```solidity
contract Vendor is Ownable {

  event BuyTokens(address buyer, uint256 amountOfETH, uint256 amountOfTokens);

  YourToken public yourToken;
  uint256 public constant tokensPerEth = 100;

  constructor(address tokenAddress) {
    yourToken = YourToken(tokenAddress);
  }

  // ToDo: create a payable buyTokens() function:
  
  function buyTokens() public payable{
    require(msg.value > 0, "Add ether to send");
    uint256 tokenWorth = tokensPerEth * msg.value;

    (bool sent) = yourToken.transfer(msg.sender, tokenWorth);
    require(sent, "Transfer failed");

    emit BuyTokens(msg.sender, msg.value, tokenWorth);
  }
}
```





