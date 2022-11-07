# Creating a Crypto Token Vendor (theweekndcoin)
I created a decentralized token that can be used for transactions through a challenge from Scaffold-Eth
This is the solution for the challenge 2 of Scaffold Eth - creating a token vendor. This particular code passes all the given 6 tests and has been accepted by [speedrunethereum.](https://speedrunethereum.com/)
> _Please do not copy this code as is, I suggest you work on your own solution and if necessary, take inspiration from the code that I've written._

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

## Understanding Vendor.sol for buyTokens() function
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

In this contract, I made the buyTokens() function a payable function which essentially transfers the required amount of tokens from the Vendor to the user's wallet. Once that is done, the function emits the event BuyTokens (This event has been declared earlier in the code).

## withdraw() function

```solidity
function withdraw() public onlyOwner { 
    (bool sent, bytes memory data) = msg.sender.call{value: address(this).balance}("");
    require(sent, "Failed to send user balance back to the owner");
  }
```

The withdraw function allows only the owner of the vendor contract to withdraw the funds. So we use onlyOwner modifier from Ownable to make sure that the user who chooses to withdraw is owner only. If the address user is not the owner then we display an error.

## sellTokens(uint256 _amount) function

```solidity
function sellTokens(uint256 _amount) public {
    require(_amount > 0, "Please input higher number of tokens");

    (bool sent) = yourToken.transferFrom(msg.sender, address(this), _amount);
    require(sent, "Failed to transfer tokens from user to vendor");

    uint256 amountOfETHToTransfer = _amount / tokensPerEth;

    (sent,) = msg.sender.call{value: amountOfETHToTransfer}("");
    require(sent, "Failed to send ETH to the user");
  }
```

The sellTokens() function transfers the tokens from the user to the vendor. If tokens are owned by the user, the vendor can accept the payment else there will be an error displayed.

That's it! That's all the code that passes all tests. If you enjoyed that, and want to show me some support, you can send me ETH at 0x04b24656E4B114e4eF83f40a1161d1804e684D89.
