# 📜 Solidity Getting Started

Your first steps into smart contract development with Solidity.

## 🔧 Environment Setup

### Option 1: Remix IDE (Recommended for Beginners)

1. Go to https://remix.ethereum.org
2. Create a new file: `contracts/MyContract.sol`
3. Start coding!

### Option 2: Hardhat (For Real Projects)

```bash
# Create project
mkdir my-project && cd my-project
npm init -y
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox

# Initialize
npx hardhat init

# Compile
npx hardhat compile

# Deploy
npx hardhat run scripts/deploy.js --network sepolia
```

### Option 3: Foundry (Fast & Modern)

```bash
# Install
curl -L https://foundry.paradigm.xyz | bash
foundryup

# Create project
forge init my-project
forge build
forge test
```

## 📝 Your First Smart Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract HelloWorld {
    string public greeting = "Hello, World!";
    
    function getGreeting() public view returns (string memory) {
        return greeting;
    }
    
    function setGreeting(string memory _newGreeting) public {
        greeting = _newGreeting;
    }
}
```

## 🏗️ Contract Structure

```solidity
// 1. License identifier
// SPDX-License-Identifier: MIT

// 2. Pragma - compiler version
pragma solidity ^0.8.20;

// 3. Contract declaration
contract ContractName {
    // State variables
    uint256 public myNumber;
    address public owner;
    
    // Events
    event NumberChanged(uint256 old, uint256 new_);
    
    // Constructor (runs once on deployment)
    constructor() {
        owner = msg.sender;
    }
    
    // Functions
    function setNumber(uint256 _num) public {
        uint256 old = myNumber;
        myNumber = _num;
        emit NumberChanged(old, _num);
    }
}
```

## 📦 Solidity Data Types

### Basic Types
```solidity
// Integers
uint256 public maxUint = type(uint256).max;  // Large positive
int256 public minInt = -1;                    // Can be negative

// Address (holds Ethereum addresses)
address public user = 0x1234...;

// Boolean
bool public isActive = true;

// Bytes (fixed-size)
bytes32 public hash = 0x5678...;

// String
string public name = "Alice";
```

### Reference Types
```solidity
// Arrays
uint256[] public numbers;
address[] public users;

// Mappings
mapping(address => uint256) public balances;
mapping(string => uint256) public scores;

// Structs
struct Person {
    string name;
    uint256 age;
}
Person public alice = Person("Alice", 25);
```

## 🔒 Visibility Modifiers

| Modifier | Access |
|----------|--------|
| `public` | Everyone can access |
| `private` | Only this contract |
| `internal` | This + child contracts |
| `external` | Only from outside |

```solidity
contract VisibilityExample {
    uint256 public publicVar = 1;      // All access
    uint256 private privateVar = 2;    // Only this contract
    uint256 internal internalVar = 3; // This + derived
    uint256 privateVar = 4;            // Default: private
    
    function publicFunc() public {}
    function privateFunc() private {}
    function internalFunc() internal {}
    function externalFunc() external {}
}
```

## 📤 Function Types

```solidity
contract FunctionTypes {
    uint256 public result;
    
    // View: Read-only, no gas cost when called externally
    function getResult() public view returns (uint256) {
        return result;
    }
    
    // Pure: No reading or writing of state
    function add(uint256 a, uint256 b) public pure returns (uint256) {
        return a + b;
    }
    
    // Payable: Can receive ETH
    function deposit() public payable {
        // msg.value contains the ETH sent
    }
    
    // Returns multiple values
    function getValues() public pure returns (uint256, bool, address) {
        return (42, true, msg.sender);
    }
}
```

## 💰 gas And msg

```solidity
contract MsgExamples {
    address public sender = msg.sender;    // Who called function
    uint256 public value = msg.value;       // ETH sent (in wei)
    bytes   public data = msg.data;         // Input data
    uint256 public gasLeft = gasleft();     // Remaining gas
}
```

## 🎯 Deploy on Testnet

### Using Remix
1. Select "Injected Provider" (MetaMask)
2. Connect MetaMask
3. Switch to Sepolia Testnet
4. Click "Deploy"

### Get Test ETH (Sepolia)
- https://sepoliafaucet.com
- https://cloud.google.com/application-layer-1-0/faucet/ethereum

## 📖 Basic Example: Simple Storage

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleStorage {
    uint256 public storedData;
    
    event Stored(uint256 value);
    
    function set(uint256 x) public {
        storedData = x;
        emit Stored(x);
    }
    
    function get() public view returns (uint256) {
        return storedData;
    }
}
```

## 🧪 Test Your Contract

```javascript
// test/Storage.test.js (Hardhat)
const { expect } = require("chai");

describe("SimpleStorage", function () {
  it("should store a value", async function () {
    const Storage = await ethers.getContractFactory("SimpleStorage");
    const storage = await Storage.deploy();
    await storage.deployed();

    await storage.set(42);
    expect(await storage.get()).to.equal(42);
  });
});
```

## 📚 Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `Stack too deep` | Too many local vars | Use struct |
| `Out of gas` | Function too complex | Optimize code |
| `Type not convertible` | Wrong type used | Cast properly |
| `Require failed` | Condition not met | Check inputs |

## 💡 Best Practices

1. **Always specify license:** `// SPDX-License-Identifier: MIT`
2. **Use recent compiler:** `^0.8.20`
3. **Check inputs:** Use `require()`
4. **Emit events:** For important state changes
5. **Handle decimals:** Solidity has no decimals!

## 📝 Practice Challenges

1. Create a contract that stores your name
2. Create a counter that increments
3. Create a contract that transfers ETH
4. Create a simple voting contract

## 🔗 Resources

- [Solidity Docs](https://docs.soliditylang.org/)
- [CryptoZombies](https://cryptozombies.io/)
- [OpenZeppelin Contracts](https://docs.openzeppelin.com/contracts/)


[← Back to Solidity Basics](../README.md) | [Next: Data Types →](data-types.md)
