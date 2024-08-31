# DeFi Empire: A Blockchain-Based Gaming Experience

**A DeFi Kingdom clone on the Avalanche network that merges decentralized finance and gaming into an immersive universe.**

## Overview

DeFi Empire is an exciting blockchain-based gaming experience where players can collect, build, and battle with their digital assets. Through various in-game activities, players can earn rewards. This project is a clone of DeFi Kingdom on the Avalanche network, blending decentralized finance (DeFi) with gaming to create a unique and engaging experience.

## Prerequisites

Before starting, ensure you have the following tools and resources:

- **Unix Computer** (MacOS or Linux) or **WSL** (Windows Subsystem for Linux) installed on Windows
- **Remix IDE** (online version)
- **MetaMask** wallet extension
- **Web Browser**
- **Kali Linux** (preferred, but optional)

## Project Steps

### 1. Set Up Your EVM Subnet

To create a custom EVM subnet on the Avalanche network, follow the instructions provided in the guide and refer to the [Avalanche documentation](https://docs.avax.network/). Setting up a custom EVM subnet will allow you to deploy your smart contracts with low fees and create a custom token.

### 2. Define Your Native Currency

Create a native currency to serve as the in-game currency for your DeFi Kingdom clone. This currency will be used for transactions, rewards, and various in-game activities.

### 3. Connect to MetaMask

To connect your EVM subnet to MetaMask, follow the instructions in the provided guide. Be sure to select your custom EVM subnet as the network in MetaMask.

### 4. Deploy Basic Building Blocks

Deploy foundational smart contracts for your game using Solidity and Remix. These contracts will define key game activities such as battling, exploring, and trading. Example contracts are provided below.

### 5. Test Your Application

Use Remix to interact with your deployed smart contracts. Deploy tokens, liquidity pools, and other essential components of your game. Test functionalities like battling, exploring, and trading within your DeFi Kingdom clone.

## Smart Contracts

### ERC20 Token Contract

This contract implements a standard ERC20 token, including features such as transfers, allowances, minting, and burning. Players can use this token as the in-game currency.

**ERC20.sol**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract ERC20 {
    uint public totalSupply;
    string public name = "RAJAT BODH";
    string public symbol = "RAJ";
    uint8 public decimals = 18;

    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address to, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address from, address to, uint amount) external returns (bool) {
        allowance[from][msg.sender] -= amount;
        balanceOf[from] -= amount;
        balanceOf[to] += amount;
        emit Transfer(from, to, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```

### Vault Contract

The Vault contract allows users to deposit and withdraw tokens while tracking the total supply and individual balances. It manages liquidity within the game.

**Vault.sol**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function _mint(address _to, uint _shares) private {
        totalSupply += _shares;
        balanceOf[_to] += _shares;
    }

    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares;
        balanceOf[_from] -= _shares;
    }

    function deposit(uint _amount) external {
        uint shares;
        if (totalSupply == 0) {
            shares = _amount;
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this));
        }

        _mint(msg.sender, shares);
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }
}
```

## Usage

To start your DeFi Kingdom adventure:

1. **Set Up Your EVM Subnet:**
   - Follow the guide and Avalanche documentation to set up your EVM subnet.
  
2. **Define Your Native Currency:**
   - Create your in-game currency using the ERC20 contract.
  
3. **Connect to MetaMask:**
   - Add your EVM subnet to MetaMask and connect it.

4. **Deploy the Smart Contracts:**
   - Use Remix to deploy the provided ERC20 and Vault contracts.

5. **Test the Application:**
   - Interact with the deployed contracts via Remix to simulate game activities.

## Author

- **Rajat Bodh**

## License

This project is licensed under the MIT License.
