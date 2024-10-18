# Solidity OOP Concepts - Part 2

## 3. Inheritance
- **Definition**: In Solidity, inheritance allows one contract to inherit properties and methods from another contract.
- **Purpose**: 
  - Promotes code reuse by enabling contracts to extend the functionality of existing contracts.
  - Helps create more modular and maintainable code.

### Key Concepts
- **Base Contract**: The contract from which other contracts inherit.
- **Derived Contract**: The contract that inherits from a base contract.
- **`is` Keyword**: Used to establish an inheritance relationship.

### Example: Basic Inheritance
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Base contract
contract Animal {
    string public species;

    // Constructor to initialize species
    constructor(string memory _species) {
        species = _species;
    }

    // Virtual function to allow overriding in derived contracts
    function speak() public pure virtual returns (string memory) {
        return "Animal speaks!";
    }
}

// Derived contract
contract Dog is Animal {
    // Call the base contract's constructor
    constructor() Animal("Canine") {}

    // Override the base contract's function
    function speak() public pure override returns (string memory) {
        return "Woof!";
    }
}
```
### Key Points

-   **Inheritance Relationship**: `Dog` inherits from `Animal` using the `is` keyword.
-   **Calling Base Constructor**: The derived contract's constructor calls the base constructor to set initial values.
-   **Overriding Functions**: The derived contract can override base functions using the `override` keyword.

## 4. Multilevel Inheritance

-   Solidity supports multilevel inheritance, where a contract can inherit from another derived contract.

### Example: Multilevel Inheritance
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Base contract
contract Vehicle {
    string public brand;

    constructor(string memory _brand) {
        brand = _brand;
    }
}

// Derived contract
contract Car is Vehicle {
    uint public wheels;

    constructor(string memory _brand, uint _wheels) Vehicle(_brand) {
        wheels = _wheels;
    }
}

// Further derived contract
contract ElectricCar is Car {
    uint public batteryCapacity;

    constructor(string memory _brand, uint _wheels, uint _batteryCapacity) 
        Car(_brand, _wheels) {
        batteryCapacity = _batteryCapacity;
    }
}
```
### Key Points

-   **Multiple Levels**: `ElectricCar` inherits from `Car`, which in turn inherits from `Vehicle`.
-   **Chained Constructors**: Each derived contract's constructor calls its base contract's constructor.

## 5. Multiple Inheritance

-   Solidity supports multiple inheritance, where a contract can inherit from multiple base contracts.
-   **Order of Inheritance Matters**: The order in which contracts are inherited can affect the behavior.

### Example: Multiple Inheritance
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Base contract 1
contract A {
    function foo() public pure virtual returns (string memory) {
        return "A";
    }
}

// Base contract 2
contract B {
    function foo() public pure virtual returns (string memory) {
        return "B";
    }
}

// Derived contract
contract C is A, B {
    // Overriding foo() to specify which base implementation to use
    function foo() public pure override(A, B) returns (string memory) {
        return super.foo(); // Calls the implementation from contract A
    }
}
```
### Key Points

-   **Overriding and `super`**: When multiple base contracts have the same function, the derived contract must explicitly override it.
-   **Linearization**: Solidity follows a specific order (C3 linearization) to resolve method calls in multiple inheritance.

## Summary
-   **Inheritance**: Allows contracts to reuse code and extend functionality.
-   **Multilevel Inheritance**: Enables a chain of inheritance.
-   **Multiple Inheritance**: Allows a contract to inherit from multiple base contracts.
-   **Override Mechanisms**: Use `override` and `super` to control function resolution.
