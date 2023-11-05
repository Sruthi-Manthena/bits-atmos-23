# bits-atmos-23
BlockSoc Workshop

# Data Storage

```solidity
// SPDX-License-Identifier: MIT
pragma solidity >= 0.8.0;

contract DataLocationExample {
    uint256 public storedValue;  // Storage variable

    constructor(uint256 initialValue) {
        storedValue = initialValue;
    }

    function updateValue(uint256 newValue) public {
        storedValue = newValue;  // Data is stored in storage
    }

    function addAndStore(uint256 a, uint256 b) public pure returns (uint256 result) {
        result = a + b;  // Data is stored in memory
    }

    function getSumFromExternal(uint256[] calldata values) public pure returns (uint256) {
        uint256 sum = 0;
        for (uint256 i = 0; i < values.length; i++) {
            sum += values[i];  // Data is in calldata
        }
        return sum;
    }

    function updateMultiple(uint256[] memory data) public {
        for (uint256 i = 0; i < data.length; i++) {
            storedValue += data[i];  // Data is in memory
        }
    }
}
```

# Solidity Function Visibility Modifiers

## Public

A `public` function can be accessed by anyone, both externally and internally. It is often used for functions that need to be accessible from outside the contract, such as setters and getters for state variables.

```solidity
contract VisibilityExample {
    uint256 publicVariable;

    function setPublicVariable(uint256 _value) public {
        publicVariable = _value;
    }

    function getPublicVariable() public view returns (uint256) {
        return publicVariable;
    }
}

```
## Internal
An `internal` function can only be accessed from within the same contract. It is not accessible from other contracts or externally. internal functions are used for encapsulating logic intended for internal use within the contract.

```solidity
contract VisibilityExample {
    uint256 internalVariable;

    function setInternalVariable(uint256 _value) internal {
        internalVariable = _value;
    }

    function getInternalVariable() public view returns (uint256) {
        return internalVariable;
    }
}

```

## Private
A private function is the most restrictive modifier, only callable from within the same contract. It cannot even be called by other functions within the same contract. private functions are used for hiding internal implementation details.

```solidity
contract VisibilityExample {
    uint256 privateVariable;

    constructor() {
        privateVariable = 10;
    }

    function setPrivateVariable(uint256 _value) private {
        privateVariable = _value;
    }

    function getPrivateVariable() public view returns (uint256) {
        return privateVariable;
    }
}
```

## View
The view modifier indicates that a function does not modify the contract's state. It is read-only and can be called both externally and internally. view functions are often used for querying contract state.

```solidity
contract VisibilityExample {
    uint256 publicVariable;

    function getDouble(uint256 _value) public view returns (uint256) {
        return _value * 2;
    }
}
```

## Pure
The pure modifier indicates that a function is purely computational and doesn't access or modify the contract's state. It is also read-only and can be used for mathematical or utility functions.

```solidity
contract VisibilityExample {
    function add(uint256 a, uint256 b) public pure returns (uint256) {
        return a + b;
    }
}
```

# Inheritence and Abstraction

```solidity
// SPDX-License-Identifier: MIT
pragma solidity >= 0.8.0;

// Parent contract
abstract contract Animal {
    string public name;

    constructor(string memory _name) {
        name = _name;
    }

    function speak() public virtual pure returns (string memory);
}

// Child contract inheriting from Animal
contract Dog is Animal {
    constructor(string memory _name) Animal(_name) {}

    function speak() public pure override returns (string memory) {
        return "Woof!";
    }
}

// Child contract inheriting from Animal
contract Cat is Animal {
    constructor(string memory _name) Animal(_name) {}

    function speak() public pure override returns (string memory) {
        return "Meow!";
    }
}
```

# Randomness
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract BasicRandomizer {
    uint256 public randomValue;

    function getRandomNumber() public {
        uint256 seed = uint256(keccak256(abi.encodePacked(block.timestamp, block.difficulty, msg.sender)));
        randomValue = uint256(keccak256(abi.encodePacked(blockhash(block.number - 1), seed, block.coinbase)));
    }
}
```
