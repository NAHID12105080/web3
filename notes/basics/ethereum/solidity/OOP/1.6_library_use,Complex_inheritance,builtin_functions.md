# Solidity OOP Concepts - Part 6

## 15. Using Libraries
- **Definition**: Libraries in Solidity are collections of reusable code that can be called by other contracts.
- **Characteristics**:
  - Cannot hold state variables.
  - Can only have functions that are `pure` or `view`.
  - Cannot inherit from other contracts or libraries.
  - Their functions are executed in the context of the calling contract, meaning they can access and modify the state of the calling contract.

### Example: Creating and Using a Library
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Define a library with some utility functions
library MathLibrary {
    function add(uint a, uint b) public pure returns (uint) {
        return a + b;
    }

    function multiply(uint a, uint b) public pure returns (uint) {
        return a * b;
    }
}

contract Calculator {
    // Using the library functions in the Calculator contract
    function calculateSum(uint x, uint y) public pure returns (uint) {
        return MathLibrary.add(x, y);
    }

    function calculateProduct(uint x, uint y) public pure returns (uint) {
        return MathLibrary.multiply(x, y);
    }
}
```
### Key Points

-   **Reusable Functions**: Libraries enable code reuse across multiple contracts.
-   **Linked to Contracts**: Library functions are accessed by referencing the library's name.

## 16. Handling Complex Inheritance

-   Solidity allows for complex inheritance structures with multiple levels of inheritance and multiple base contracts.
-   **Linearization of Inheritance**: Solidity uses C3 linearization to resolve the order in which base contracts are inherited.

### Example: Diamond Inheritance Problem
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract A {
    function foo() public pure virtual returns (string memory) {
        return "A";
    }
}

contract B is A {
    function foo() public pure virtual override returns (string memory) {
        return "B";
    }
}

contract C is A {
    function foo() public pure virtual override returns (string memory) {
        return "C";
    }
}

contract D is B, C {
    function foo() public pure override(B, C) returns (string memory) {
        return super.foo(); // Calls C's implementation of foo
    }
}
```
### Key Points

-   **Diamond Problem Solution**: Solidity resolves inheritance order using C3 linearization, where functions from the most-derived contract are called first.
-   **Override and Super Keywords**: Use these to explicitly specify which base contract's function to call.

## 17. Solidity's Built-in Functions

Solidity provides several built-in functions and global variables that can be used in contracts.

### Commonly Used Built-in Functions

1.  **`address.transfer(uint256 amount)`**: Sends Ether to the specified address.
2.  **`address.send(uint256 amount)`**: Sends Ether to the specified address but returns `false` if it fails.
3.  **`address.call(bytes memory data)`**: Calls a function on another contract. Returns a boolean indicating success or failure.
4.  **`selfdestruct(address payable recipient)`**: Destroys the contract and sends the remaining Ether to the specified address.

### Commonly Used Global Variables

1.  **`msg.sender`**: The address of the caller of the current function.
2.  **`msg.value`**: The amount of Ether sent with the transaction.
3.  **`block.timestamp`**: The current block's timestamp.
4.  **`block.number`**: The current block number.

### Example: Using Built-in Functions
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract BuiltInExample {
    address payable public owner;

    constructor() {
        owner = payable(msg.sender);
    }

    // Function to transfer Ether to another address
    function transferEther(address payable recipient, uint amount) public {
        require(msg.sender == owner, "Only the owner can transfer Ether");
        require(address(this).balance >= amount, "Insufficient balance");
        recipient.transfer(amount);
    }

    // Function to get the current block timestamp
    function getBlockTimestamp() public view returns (uint) {
        return block.timestamp;
    }
}
```
### Key Points

-   **Global Variables**: Useful for accessing transaction and blockchain-related data.
-   **Built-in Functions**: Provide utility for handling Ether transfers and interacting with other contracts.

## Summary

-   **Libraries**: Facilitate code reuse and help keep contracts modular.
-   **Complex Inheritance**: Handled using C3 linearization, with explicit use of `override` and `super`.
-   **Built-in Functions and Variables**: Provide essential tools for interacting with the blockchain.
