---
meta:
  - name: "title"
    content: Hazlor | Hazlor EVM Chain | Deploy Smart Contract
  - name: "description"
    content: Learn how to deploy a smart contract to Hazlor using Solidity, both Truffle and Hardhat are included in this technical documentation.
  - name: "og:title"
    content: Hazlor | Hazlor EVM Chain | Deploy Smart Contract
  - name: "og:type"
    content: Website
  - name: "og:description"
    content: Learn how to deploy a smart contract to Hazlor using Solidity, both Truffle and Hardhat are included in this technical documentation.
  - name: "og:image"
    content: https://cronos.crypto.org/og-image.png
  - name: "twitter:title"
    content: Hazlor | Hazlor EVM Chain | Deploy Smart Contract
  - name: "twitter:site"
    content: "@cryptocom"
  - name: "twitter:card"
    content: summary_large_image
  - name: "twitter:description"
    content: Learn how to deploy a smart contract to Hazlor using Solidity, both Truffle and Hardhat are included in this technical documentation.
  - name: "twitter:image"
    content: https://cronos.crypto.org/og-image.png
canonicalUrl: https://cronos.crypto.org/docs/getting-started/cronos-smart-contract.html
---

# Hazlor: Deploy Smart Contract

This documentation demostrate the deployment of smart contract to Hazlor, using Solidity. `@openzeppelin/contracts` is used for the demo sodlity script.

Both Truffle and Hardhat are included in this documentation, feel free to choose one of them.

## Pre-requisites

### Supported OS

We officially support macOS, Windows and Linux only. Other platforms may work but there is no guarantee. We will extend our support to other platforms after we have stabilized our current architecture.

### Prepare nodejs and npm environment 

You can refer to [Downloading and installing Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)  
`Nodejs v10` is suggested 

### Sufficient fund on deployer address
You can access to [faucet](https://cronos.crypto.org/faucet) and [explorer](https://cronos-explorer.crypto.org/) to obtain testnet TCRO.

### Git clone `smart-contract-example`
  ```bash
  $ git clone git@github.com:hazlorlabs/core-smart-contract-example.git
  ```

## Truffle: Deploy ERC20 Contract

### Step 1. Enter `smart-contract-example/truffle` folder
  ```bash
  $ cd cronos-smart-contract-example/truffle
  ```

### Step 2. Run `npm install` inside the folder
  ```bash
  $ npm install
  ```

### Step 3. Make a copy of `.env.example` to `.env`
  ```bash
  $ cp .env.example .env
  ```

### Step 4. Modify `.env` and fill *ONE* of the field
  ```bash
  MNEMONIC=goose easy ivory ...
  PRIVATE_KEY=XXXXXXX
  ```

### Step 5. Review Migration Script at `migrations/2_deploy_cronos_token.js`
  ```javascript
    const CronosToken = artifacts.require("CronosToken");
    
    module.exports = function (deployer) {
        deployer.deploy(CronosToken, "Hazlor Token", "CRT", "1000000000000000000000000");
    };
  ```
  

### Step 6. Endpoints setting
By default, the script will be using your local host `"127.0.0.1"`  - If you are not running a localhost, you may leverage the public endpoint `https://cronos-testnet-3.crypto.org` by making changes to `networks` in `truffle-config.js`, for example:

```json
  networks: {
    development: {
     host: "https://cronos-testnet-3.crypto.org",     
     port: 8545,            
     network_id: "*",       
    },
    cronos: {
      provider: new HDWalletProvider(getHDWallet(), "https://cronos-testnet-3.crypto.org:8545"), 
      network_id: "*",
      skipDryRun: true
    },
  },
```

### Step 7. Deploy Contract
  ```bash
  npm run deploy-contract-cronos
  ```

### Step 8. Obtain Contract address from console and input to Metamask
Correct balance will be shown on Metamask page
<img src="./assets/cronos-smart-contract/truffle_deploy_contract_address.png" />
<img src="./assets/cronos-smart-contract/metamask_add_tokens.png" />
<img src="./assets/cronos-smart-contract/metamask_add_token_success.png" />

## Hardhat: Deploy ERC20 Contract
### Step 1. Enter `smart-contract-example/hardhat` folder
  ```bash
  $ cd smart-contract-example/hardhat
  ```

### Step 2. Run `npm install` inside the folder
  ```bash
  $ npm install
  ```

### Step 3. Make a copy of `.env.example` to `.env`
  ```bash
  $ cp .env.example .env
  ```

### Step 4. Modify `.env` and fill *ONE* of the field
  ```bash
  MNEMONIC=goose easy ivory ...
  PRIVATE_KEY=XXXXXXX
  ```

### Step 5. Review Migration Script at `scripts/deploy-cronos-token.js`
  ```javascript
    async function main() {
        const CronosToken = await hre.ethers.getContractFactory("CronosToken");
        const cronosToken = await CronosToken.deploy("Hazlor Token", "CRT", "1000000000000000000000000");
    
        await cronosToken.deployed();
    
        console.log("CronosToken deployed to:", cronosToken.address);
    }
  ```

### Step 6. Endpoints setting
By default, the script will be using your local host `"127.0.0.1"`  - If you are not running a localhost, you may leverage the public endpoint `https://cronos-testnet-3.crypto.org` by making changes to `networks` in `truffle-config.js`, for example:

```json
  networks: {
    development: {
     host: "https://cronos-testnet-3.crypto.org",     
     port: 8545,            
     network_id: "*",       
    },
    cronos: {
      provider: new HDWalletProvider(getHDWallet(), "https://cronos-testnet-3.crypto.org:8545"), 
      network_id: "*",
      skipDryRun: true
    },
  },
```
### Step 7. Deploy Contract
  ```bash
  npm run deploy-contract-cronos
  ```

### Step 8. Obtain Contract address from console and input to Metamask
Correct balance will be shown on Metamask page
  ```bash
  CronosToken deployed to: 0x5F803c894a0A16B46fe5982fB5D89eb334eAF68
  ```
<img src="./assets/cronos-smart-contract/metamask_add_tokens.png" />
<img src="./assets/cronos-smart-contract/metamask_add_token_success.png" />
