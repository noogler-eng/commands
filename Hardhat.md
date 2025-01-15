# Solidity Smart Contract Development with Hardhat

A comprehensive guide for developing, testing, and deploying smart contracts using Solidity and Hardhat.

## Features

- Hardhat development environment setup
- Solidity smart contract development
- Advanced testing framework
- Contract deployment scripts
- Gas optimization tools
- Mainnet forking capabilities
- TypeScript support
- OpenZeppelin integration

## Prerequisites

- Node.js (v14.0.0 or higher)
- npm or yarn package manager
- Git
- Code editor (VS Code recommended with Solidity extension)

## Project Setup

1. Create a new project:
```bash
mkdir my-smart-contracts
cd my-smart-contracts
npm init -y
```

2. Install Hardhat and dependencies:
```bash
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox @nomicfoundation/hardhat-network-helpers @nomicfoundation/hardhat-chai-matchers @nomiclabs/hardhat-ethers @nomiclabs/hardhat-etherscan chai ethers hardhat-gas-reporter solidity-coverage @typechain/hardhat typechain @typechain/ethers-v6 @types/chai @types/node @types/mocha typescript
```

3. Initialize Hardhat project:
```bash
npx hardhat init
```

## Project Structure

```
├── contracts/
│   ├── Token.sol
│   └── NFT.sol
├── scripts/
│   ├── deploy.ts
│   └── interact.ts
├── test/
│   └── Token.test.ts
├── hardhat.config.ts
├── .env
├── .gitignore
└── package.json
```

## Configuration

Create `hardhat.config.ts`:

```typescript
import { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";
import dotenv from "dotenv";

dotenv.config();

const config: HardhatUserConfig = {
  solidity: {
    version: "0.8.19",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200
      }
    }
  },
  networks: {
    hardhat: {},
    sepolia: {
      url: `https://sepolia.infura.io/v3/${process.env.INFURA_KEY}`,
      accounts: process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : []
    },
    mainnet: {
      url: `https://mainnet.infura.io/v3/${process.env.INFURA_KEY}`,
      accounts: process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : []
    }
  },
  etherscan: {
    apiKey: process.env.ETHERSCAN_API_KEY
  },
  gasReporter: {
    enabled: true,
    currency: "USD",
    coinmarketcap: process.env.COINMARKETCAP_API_KEY
  }
};

export default config;
```

## Example Smart Contract

```solidity
// contracts/Token.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract Token is ERC20, Ownable {
    uint256 public constant INITIAL_SUPPLY = 1000000 * 10**18;

    constructor() ERC20("MyToken", "MTK") {
        _mint(msg.sender, INITIAL_SUPPLY);
    }

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }
}
```

## Deployment Script

```typescript
// scripts/deploy.ts
import { ethers } from "hardhat";

async function main() {
  const Token = await ethers.getContractFactory("Token");
  console.log("Deploying Token...");
  
  const token = await Token.deploy();
  await token.deployed();

  console.log("Token deployed to:", token.address);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

## Testing

Create test files in the `test` directory:

```typescript
// test/Token.test.ts
import { expect } from "chai";
import { ethers } from "hardhat";
import { Token } from "../typechain-types";
import { SignerWithAddress } from "@nomiclabs/hardhat-ethers/signers";

describe("Token", function () {
  let token: Token;
  let owner: SignerWithAddress;
  let addr1: SignerWithAddress;

  beforeEach(async function () {
    [owner, addr1] = await ethers.getSigners();
    const Token = await ethers.getContractFactory("Token");
    token = await Token.deploy();
    await token.deployed();
  });

  it("Should assign the total supply to the owner", async function () {
    const ownerBalance = await token.balanceOf(owner.address);
    expect(await token.totalSupply()).to.equal(ownerBalance);
  });

  it("Should allow owner to mint tokens", async function () {
    const mintAmount = ethers.utils.parseEther("100");
    await token.mint(addr1.address, mintAmount);
    expect(await token.balanceOf(addr1.address)).to.equal(mintAmount);
  });

  it("Should allow users to burn tokens", async function () {
    const burnAmount = ethers.utils.parseEther("100");
    await token.transfer(addr1.address, burnAmount);
    await token.connect(addr1).burn(burnAmount);
    expect(await token.balanceOf(addr1.address)).to.equal(0);
  });
});
```

## Available Scripts

Add these scripts to your `package.json`:

```json
{
  "scripts": {
    "compile": "hardhat compile",
    "test": "hardhat test",
    "coverage": "hardhat coverage",
    "deploy:sepolia": "hardhat run scripts/deploy.ts --network sepolia",
    "deploy:mainnet": "hardhat run scripts/deploy.ts --network mainnet",
    "verify": "hardhat verify",
    "node": "hardhat node",
    "clean": "hardhat clean"
  }
}
```

## Common Tasks

1. Compile contracts:
```bash
npx hardhat compile
```

2. Run tests:
```bash
npx hardhat test
```

3. Start local network:
```bash
npx hardhat node
```

4. Deploy to network:
```bash
npx hardhat run scripts/deploy.ts --network sepolia
```

5. Verify contract:
```bash
npx hardhat verify --network sepolia DEPLOYED_CONTRACT_ADDRESS
```

## Gas Optimization Tips

1. Use `unchecked` blocks for arithmetic operations when overflow is impossible
2. Pack variables efficiently
3. Use `external` instead of `public` for functions called externally
4. Avoid unnecessary storage
5. Use events instead of storage when possible

Example of optimized storage:

```solidity
contract OptimizedStorage {
    // Pack these variables together in a single storage slot
    uint128 public value1; // 16 bytes
    uint128 public value2; // 16 bytes
    
    // This will start a new storage slot
    uint256 public value3; // 32 bytes
}
```

## Security Best Practices

1. Use OpenZeppelin contracts whenever possible
2. Always use the latest stable Solidity version
3. Implement access control
4. Add emergency stops (circuit breakers)
5. Thoroughly test all functions
6. Get external audits before mainnet deployment

## Debugging

1. Use Hardhat Console:
```solidity
import "hardhat/console.sol";

contract MyContract {
    function myFunction() public {
        console.log("Value:", someValue);
    }
}
```

2. Use Hardhat Network for local debugging
3. Implement proper error messages
4. Use try/catch for external calls

## Environment Variables

Create a `.env` file:

```
INFURA_KEY=your_infura_key
PRIVATE_KEY=your_private_key
ETHERSCAN_API_KEY=your_etherscan_key
COINMARKETCAP_API_KEY=your_coinmarketcap_key
```

## Resources

- [Hardhat Documentation](https://hardhat.org/getting-started/)
- [Solidity Documentation](https://docs.soliditylang.org/)
- [OpenZeppelin Contracts](https://docs.openzeppelin.com/contracts/)
- [Ethereum Security Best Practices](https://consensys.github.io/smart-contract-best-practices/)

## License

This project is licensed under the MIT License.
