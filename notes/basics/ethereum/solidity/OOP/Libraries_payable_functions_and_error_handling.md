# Solidity OOP Concepts - Part 4
## 9. Libraries
 **Definition**: Libraries are similar to contracts but are used to add reusable code that can be shared across multiple contracts.
**Characteristics**:
 - Libraries cannot hold state, meaning they do not have state variables.
 - They can only contain functions that are `pure` or `view`.
 - Cannot inherit or be inherited.
 - Library functions are executed in the context of the calling contract.

### Example: Using a Library
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Library for basic arithmetic operations
library MathLib {
    function add(uint a, uint b) public pure returns (uint) {
        return a + b;
    }

    function multiply(uint a, uint b) public pure returns (uint) {
        return a * b;
    }
}

contract Calculator {
    // Using the MathLib library
    function calculateSum(uint x, uint y) public pure returns (uint) {
        return MathLib.add(x, y);
    }

    function calculateProduct(uint x, uint y) public pure returns (uint) {
        return MathLib.multiply(x, y);
    }
}
``` 

### Key Points

-   **Stateless**: Libraries are used for reusable, stateless utility functions.
-   **Linked to Contracts**: Library functions can be called directly in the contract using the library's name.

## 10. Payable Functions and Addresses

-   **Definition**: Payable functions allow a contract to receive Ether. A function must be marked as `payable` to accept payments.
-   **Payable Address**: A type of address that can hold Ether and participate in financial transactions.

### Example: Using Payable Functions

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Payment {
    address payable public owner;

    // Set the owner as the contract deployer
    constructor() {
        owner = payable(msg.sender);
    }

    // Payable function to receive Ether
    function deposit() public payable {
        require(msg.value > 0, "Must send some Ether");
    }

    // Function to withdraw Ether to the owner's address
    function withdraw(uint amount) public {
        require(msg.sender == owner, "Only the owner can withdraw");
        require(amount <= address(this).balance, "Insufficient balance");
        owner.transfer(amount);
    }

    // Function to check contract balance
    function getBalance() public view returns (uint) {
        return address(this).balance;
    }
}
``` 

### Key Points

-   **Marking Functions as Payable**: Required to enable Ether transfers.
-   **Financial Transactions**: Contracts can send and receive Ether using payable addresses.

## 11. Error Handling (Revert and Require)

-   **Purpose**: Error handling mechanisms in Solidity provide ways to validate conditions and revert transactions if certain criteria are not met.
-   **Revert**: Unconditionally aborts a transaction, rolls back changes, and refunds the remaining gas.
-   **Require**: Evaluates a condition and throws an error if the condition is false.

### Example: Error Handling Using Require and Revert

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ErrorHandling {
    error InsufficientBalance(uint balance, uint requested);

    // Function to check for sufficient balance before withdrawal
    function withdraw(uint amount) public view {
        uint balance = address(this).balance;
        if (balance < amount) {
            revert InsufficientBalance(balance, amount);
        }
    }

    // Function to check conditions using require
    function transfer(address payable recipient, uint amount) public payable {
        require(msg.value == amount, "The value sent must match the amount");
        recipient.transfer(amount);
    }
}
``` 

### Key Points

-   **Require Statements**: Used to check conditions before executing code.
-   **Revert with Error Messages**: Allows custom error definitions and error messages for better debugging.

## Summary

-   **Libraries**: Provide reusable, stateless utility functions to enhance modularity.
-   **Payable Functions**: Allow contracts to handle Ether transactions securely.
-   **Error Handling**: Use `require` and `revert` for validating conditions and managing errors.
