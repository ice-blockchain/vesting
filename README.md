# Vesting Frontend

## Introduction

The **Vesting Frontend** is a web application that allows users to interact with vesting contracts on the ION network. It provides a user-friendly interface for managing vesting schedules, checking smart contracts, and integrating with various third-party contracts and APIs.

This project is a fork of the original Vesting Frontend, modified to integrate with the **ION network**. This documentation describes how the vesting webpage integrates with different third-party contracts and APIs and provides guidance on setting up and using the application.

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Integration with Third-Party Contracts and APIs](#integration-with-third-party-contracts-and-apis)
    - [Vesting Smart Contracts](#vesting-smart-contracts)
    - [TON Web](#ton-web)
    - [Smart Contract Checking](#smart-contract-checking)
- [Setup and Installation](#setup-and-installation)
    - [Prerequisites](#prerequisites)
    - [Installation](#installation)
- [Usage](#usage)
- [Development](#development)
    - [Project Structure](#project-structure)
    - [Contributing](#contributing)
- [License](#license)

## Features

- **Vesting Schedule Management**: Create and manage vesting schedules on the ION network.
- **Smart Contract Interaction**: Interact with vesting smart contracts to facilitate automated token vesting.
- **Smart Contract Verification**: Verify smart contracts before adding them to the whitelist.
- **Third-Party Integration**: Seamless interaction with various third-party contracts and APIs.

## Integration with Third-Party Contracts and APIs

### Vesting Smart Contracts

The Vesting Frontend interacts with specific versions of vesting smart contracts deployed on the ION network. The contracts' codes and code hashes are detailed in the [CONTRACT-VERSIONS.txt](./CONTRACT-VERSIONS.txt) file.

Supported contract versions:

- **vesting-v0.98**: Beta version, **not for production**.
- **vesting-v0.99**: Beta version, **not for production**.
- **vesting-v1.00**: Release version.

Each contract version includes:

- `code`: The bytecode of the contract.
- `code_hash`: The hash of the contract code.

The frontend uses these details to interact with the correct versions of the vesting contracts, ensuring compatibility and security.

### TON Web

The application utilizes the [TON Web](https://github.com/toncenter/tonweb) library for interacting with the TON blockchain, which the ION network is based upon. TON Web provides tools to work with smart contracts, wallets, and messages.

Key integrations:

- **Wallet Interaction**: Managing wallets and transactions on the blockchain.
- **Contract Interaction**: Deploying and interacting with smart contracts.
- **Provider API**: Communicating with the blockchain nodes via HTTP requests.

By integrating TON Web, the Vesting Frontend can seamlessly interact with the ION network, leveraging the existing infrastructure for blockchain operations.

### Smart Contract Checking

The frontend includes functionality to check smart contracts before they are added to the whitelist. This ensures that only valid and authorized contracts are interacted with, enhancing security and reliability.

The script [check-smart-contract.js](./js/check-smart-contract.js) provides methods to:

- **Identify Contract Type**: Determine if a contract is a recognized pool or nominator contract based on its code hash.
- **Retrieve Contract Data**: Fetch essential data from the contract, such as owner and validator addresses.
- **Validate Contract Integrity**: Compute and compare code hashes to ensure the contract has not been tampered with.

#### Supported Contract Types

- **Nominator Pool Contracts**: Identified by specific code hashes, these contracts represent pools where nominators stake together.
- **Single Nominator Contracts**: Versions 1.0 and 1.1 are supported, each identified by their unique code hashes.

#### Checking Procedure

1. **Retrieve Account Information**: Use the TON provider API to get the account's state and code.
2. **Determine Account State**: Check if the account is initialized, uninitialized, or frozen.
3. **Compare Code Hashes**: Calculate the code hash of the contract and compare it with known hashes.
4. **Validate Contract Data**: For recognized contracts, verify specific data like owner and validator addresses.
5. **Return Status and Message**: Provide a status (`SUCCESS`, `INVALID`, `NO_RESPONSE`) and a descriptive message based on the checks.

#### Example Usage

```javascript
const tonweb = new TonWeb(new TonWeb.HttpProvider('https://ion.toncenter.com/api/v2/jsonRPC'));
const address = new TonWeb.utils.Address('EQD...'); // Replace with the actual address

checkSmartContract(tonweb, address).then(result => {
  console.log(result.status); // e.g., 'SUCCESS'
  console.log(result.text);   // Detailed message about the contract
});
```

## Setup and Installation

### Prerequisites

- **Node.js**: Version 14.x or higher.
- **npm**: Node.js package manager.
- **Git**: Version control system.

### Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/yourusername/vesting-frontend.git
   cd vesting-frontend
   ```

2. **Install Dependencies**

   ```bash
   npm install
   ```

3. **Start the Development Server**

   ```bash
   npm run start
   ```

   The application will be available at `http://localhost:8080`.

## Usage

1. **Access the Frontend**

   Open your web browser and navigate to `http://localhost:8080`.

2. **Manage Vesting Schedules**

    - Use the interface to create new vesting schedules.
    - View existing schedules and their details.
    - Modify or cancel vesting schedules as needed.

3. **Check Smart Contracts**

    - Utilize the smart contract checking feature to validate contracts before adding them to the whitelist.
    - Ensure that contracts are valid and authorized, preventing unauthorized access or interactions.

## Development

### Project Structure

- **`./package.json`**

  Contains project metadata and dependencies:

  ```json
  {
    "name": "vesting",
    "version": "1.0.0",
    "scripts": {
      "start": "http-server -p 8080"
    },
    "devDependencies": {
      "http-server": "^14.1.1"
    }
  }
  ```

- **`./CONTRACT-VERSIONS.txt`**

  Details the different versions of the vesting contracts, including their codes and code hashes. It's essential for the frontend to interact with the correct contract versions.

- **`./js/check-smart-contract.js`**

  JavaScript file that contains functions to check and validate smart contracts before they are added to the whitelist.

