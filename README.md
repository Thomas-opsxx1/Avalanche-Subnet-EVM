**DeFi Empire: Create a DeFi Kingdom Clone**

Step into DeFi Empire, where you can create your own DeFi Kingdom clone on the Avalanche network. This project is perfect for blockchain gaming enthusiasts looking to explore decentralized finance (DeFi) and build an immersive gaming experience.

### Overview
In this project, you will set up an EVM (Ethereum Virtual Machine) subnet on the Avalanche network, create custom tokens, and deploy smart contracts to form the core of your DeFi Kingdom clone. By following the steps provided, you can build a digital world where players can collect assets, build, and engage in battles, earning rewards through various in-game activities.

### Avalanche Subnet Introduction 

Avalanche is a high-performance, scalable, and customizable blockchain platform designed to address the limitations of traditional blockchain networks. Launched by Ava Labs in 2020, Avalanche enables developers to build decentralized applications (dApps) and deploy custom blockchain networks with near-instant transaction finality, low fees, and high throughput.

An Avalanche Subnet (short for "subnetwork") is a customizable and independent blockchain network within the Avalanche ecosystem. Subnets allow developers or organizations to create and operate their own blockchain (or blockchains) with unique rules, governance, and functionality, while benefiting from Avalanche’s core platform features like scalability, security, and interoperability.

### Getting Started
Follow these steps to begin building your DeFi Empire:

1. **Set up your EVM subnet**: Follow our guide and the Avalanche documentation to create a custom EVM subnet on the Avalanche network. This subnet will act as the foundation for deploying your smart contracts.

2. **Define your native currency**: Set up your native currency, which will function as the in-game currency for your DeFi Kingdom clone. This currency will be used for transactions within the game.

3. **Connect to Metamask**: Link your EVM subnet to Metamask to enable interactions with your smart contracts. Use our guide to integrate Metamask with your custom subnet seamlessly.

4. **Deploy the building blocks**: Use Solidity and Remix to deploy the essential smart contracts for your game, including contracts for battling, exploring, and trading. You will use an ERC20 token contract and a Vault contract as the foundation of your DeFi Kingdom clone.

### Smart Contracts

**ERC20 Token Contract**  
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "Solidity by Example";
    string public symbol = "SOLBYEX";
    uint8 public decimals = 18;

		event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
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

This ERC20 token contract is used to create and manage fungible tokens within your game. Players will use these tokens for in-game transactions and various activities.

**Vault Contract**  
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function totalSupply() external view returns (uint);

    function balanceOf(address account) external view returns (uint);

    function transfer(address recipient, uint amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool);

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
        /*
        a = amount
        B = balance of token before deposit
        T = total supply
        s = shares to mint

        (T + s) / T = (a + B) / B 

        s = aT / B
        */
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
        /*
        a = amount
        B = balance of token before withdraw
        T = total supply
        s = shares to burn

        (T - s) / T = (B - a) / B 

        a = sB / T
        */
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }
}
```

The Vault contract allows players to deposit and withdraw tokens securely. It also includes minting and burning functionalities to manage the game’s token supply.

### Challenge: Build Your DeFi Empire

Take on the challenge of creating your DeFi Empire by using the provided resources and guidelines. Begin your journey to build a thriving decentralized gaming ecosystem on the Avalanche network!
