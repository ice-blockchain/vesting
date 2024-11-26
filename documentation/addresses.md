## Smart Contract Addresses Used in the Project

This project interacts with various smart contracts on the **TON network** to facilitate vesting functionalities. Below is a detailed description of the smart contract addresses and code hashes used within the project. Understanding these addresses is crucial, especially if you plan to reconfigure them to your own contracts.

### Special Addresses

#### Elector Contract

- **Address**: `-1:3333333333333333333333333333333333333333333333333333333333333333`

  The Elector contract is a special system contract on the ION network's masterchain. It is responsible for managing validator elections and is essential for network consensus. In the context of this project, the Elector address is referenced to ensure that vesting wallets interact correctly with election-related functionalities.

  **Note**: Vesting wallets must be in the masterchain to interact with the Elector contract.

#### Config Contract

- **Address**: `-1:5555555555555555555555555555555555555555555555555555555555555555`

  The Config contract holds the network configuration parameters for the ION network. It provides essential information such as workchain settings, gas prices, and other network-level configurations. While the Config contract is vital for network operations, there's no need to add it to the whitelist within this project.

### Vesting Smart Contracts

The Vesting Frontend interacts with specific versions of vesting smart contracts deployed on the ION network. These contracts are critical for managing vesting schedules and ensuring tokens are vested according to predefined conditions.

The contract versions used in this project are:

- **vesting-v0.98**: Beta version (**not for production**).
- **vesting-v0.99**: Beta version (**not for production**).
- **vesting-v1.00**: Release version (**recommended for production**).

These contracts are defined by their **code** and **code hash**. The code hash acts as a unique identifier for the contract's code, ensuring that the frontend interacts with the correct contract version.

#### Contract Codes and Code Hashes

The detailed code and code hashes for each version are provided in the [CONTRACT-VERSIONS.txt](./CONTRACT-VERSIONS.txt) file.

For example:

- **vesting-v1.00**:
    - **Code Hash**: `b48b531abec3b714638291f7d77ed6dc9f6a2729efca20477137374d4ae8b590`
    - **Description**: Release version of the vesting contract. It's recommended for production use.

### Nominator and Nominator Pool Contracts

The project includes functionality to check and interact with nominator contracts, ensuring that they are valid and authorized before being added to the whitelist.

#### Code Hashes for Nominator Contracts

- **Nominator Pool Code Hash**: `mj7BS8CY9rRAZMMFIiyuooAPF92oXuaoGYpwle3hDc8=`
    - **Description**: Used to identify Nominator Pool contracts. Adding nominator pools to the whitelist is prohibited within this project for security reasons.

- **Single Nominator v1.0 Code Hash**: `pCrmnqx2/+DkUtPU8T04ehTkbAGlqtul/B2JPmxx9bo=`
    - **Description**: Identifies Single Nominator contracts version 1.0.

- **Single Nominator v1.1 Code Hash**: `zA05WJ6ywM/g/eKEVmV6O909lTlVrj+Y8lZkqzyQT70=`
    - **Description**: Identifies Single Nominator contracts version 1.1.

These code hashes are utilized in the [`check-smart-contract.js`](./js/check-smart-contract.js) script to verify the integrity and version of nominator contracts.

### Smart Contract Verification

The `check-smart-contract.js` script contains functions to:

- **Verify Contract Type**: Determine if a contract is a recognized vesting or nominator contract based on its code hash.
- **Validate Contract Integrity**: Ensure the contract code matches the expected code hash, confirming it hasn't been tampered with.
- **Retrieve Contract Data**: Extract essential information such as owner and validator addresses.

#### Special Address Handling

In the smart contract checking process, special addresses are handled explicitly:

- **Elector Address**: Recognized by the script and provides a note that the vesting wallet must be in the masterchain.
- **Config Address**: Acknowledged in the script with no action required for whitelisting.

#### Example from `check-smart-contract.js`

```javascript
if (addressString === '-1:3333333333333333333333333333333333333333333333333333333333333333') {
    return {
        status: SUCCESS,
        text: 'Elector. Note that vesting-wallet NEED TO BE in masterchain.'
    }
}

if (addressString === '-1:5555555555555555555555555555555555555555555555555555555555555555') {
    return {
        status: SUCCESS,
        text: 'Config. No need to add it to the whitelist.'
    }
}
```

### Reconfiguring to Your Own Contracts

If you plan to reconfigure the project to use your own smart contracts:

- **Update Contract Codes and Code Hashes**: Replace the contract codes and code hashes in the [CONTRACT-VERSIONS.txt](./CONTRACT-VERSIONS.txt) file with those of your own contracts.
- **Modify Verification Scripts**: Update the code hashes in the [`check-smart-contract.js`](./js/check-smart-contract.js) script to correspond to your contracts.
- **Adjust Special Addresses**: If your network uses different special addresses for system contracts like Elector or Config, modify the address checks accordingly.

### `index.html` configuration
Below are example configuration values for `index.html` API end-points.
```js
    // TONWEB COMMON

    const BN = TonWeb.utils.BN;
    const fromNano = TonWeb.utils.fromNano;
    const toNano = TonWeb.utils.toNano;
    const TONCENTER_URL = 'https://http-api.testnet.ice.io/jsonRPC';
    const TONCENTER_INDEX_URL = 'https://index-api.testnet.ice.io/';
    const tonweb = new TonWeb(new TonWeb.HttpProvider(TONCENTER_URL));
```


Below are example configuration values for `index.html` smart contract addresses.
```js
    // STAKING COMMON

    /** @type {string} */
    const STAKING_CONTRACT_ADDRESS = IS_TESTNET ? 'kQCu_j-5niSEIN_R3qJMWvcjKSdpBJOFz1sJE9JXt549GAW8' : 'EQCkWxfyhAkim3g2DjKQQg8T5P4g-Q1-K_jErGcDJZ4i-vqR';
    /** @type {string} */
    const STAKE_TOKEN_NAME = 'tsTON';
    /** @type {string} */
    const UNSTAKE_PAYLOAD = 0x595f07bc
    /** @type {string} */
    const STAKE_PAYLOAD = 0x47d54391
    /** @type {string} */
    const REF_PAYLOAD = 0x000000106796caef
    /** @type {string} */
    const STAKING_FEE_RES = "1.5"
    /** @type {string} */
    const STAKE_FEE = "1"
    /** @type {string} */
    const UNSTAKE_FEE = "1.05"

    // ELECTOR COMMON

    /** @type {string} */
    const ELECTOR_CONTRACT_ADDRESS = 'Ef8zMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzMzM0vF';
```

