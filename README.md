# FundMe Project Documentation

This project implements a decentralized crowdfunding platform where users can send ETH to a contract and the contract owner can withdraw the funds. It utilizes **Chainlink Price Feeds** to ensure that the funds meet a minimum USD equivalent value.

---

## **Contracts Overview**

### **1. FundMe.sol**
This is the core contract that handles the crowdfunding logic. 

#### **Key Features**
- **Funders Mapping**: Tracks how much ETH each address has contributed.
- **Owner Access**: Only the contract owner can withdraw funds.
- **Price Feeds**: Uses Chainlink Price Feeds to convert ETH to USD.
- **Fallback and Receive Functions**: Allow the contract to accept ETH transfers.

#### **Key Variables**
- `s_addressToAmountFunded`: Mapping to store funders and their contributions.
- `s_funders`: Array to track all funders.
- `i_owner`: Immutable address of the contract owner.
- `MINIMUM_USD`: Minimum contribution in USD (set to $5).

#### **Key Functions**
1. `fund()`: 
   - Accepts ETH and ensures the value meets the minimum USD equivalent.
   - Updates the mapping and adds the sender to the funders array.

2. `withdraw()`: 
   - Allows the owner to withdraw all funds, resetting the funders' contributions.

3. `cheaperWithdraw()`: 
   - Optimized withdrawal function that reduces gas costs.

4. **Modifiers**
   - `onlyOwner`: Restricts access to owner-only functions.

---

### **2. PriceConverter.sol**
This library provides utility functions for converting ETH to USD.

#### **Key Functions**
1. `getPrice(AggregatorV3Interface priceFeed)`: 
   - Retrieves the current ETH/USD price using the Chainlink Price Feed.

2. `getConversionRate(uint256 ethAmount, AggregatorV3Interface priceFeed)`: 
   - Converts an ETH amount to its USD equivalent.

---

### **3. DeployFundMe.s.sol**
A Foundry script to deploy the `FundMe` contract.

#### **Key Steps**
- Deploys the `HelperConfig` contract to fetch the price feed address for the active network.
- Deploys the `FundMe` contract, passing the price feed address as a constructor argument.

---

### **4. HelperConfig.s.sol**
Handles network configurations for the `FundMe` contract.

#### **Key Features**
1. **NetworkConfig Struct**:
   - Stores the price feed address for different networks (e.g., Sepolia, Mainnet).
2. **Local Mock Deployment**:
   - Deploys a `MockV3Aggregator` for testing in local environments.
3. **Active Network Selection**:
   - Automatically selects the appropriate configuration based on the chain ID.

---

### **5. Interactions.s.sol**
Scripts to interact with the deployed `FundMe` contract.

#### **Key Contracts**
1. `FundFundMe`:
   - Sends ETH to the contract's `fund()` function.
2. `WithdrawFundMe`:
   - Calls the `withdraw()` function to retrieve funds.

---

### **6. FundMeTest.t.sol**
Unit tests for the `FundMe` contract using Foundry's `Test` library.

#### **Key Tests**
1. **Basic Contract Properties**:
   - Checks the minimum USD value and the owner's address.
2. **Fund and Withdraw**:
   - Verifies that contributions are recorded correctly and only the owner can withdraw funds.
3. **Price Feed Accuracy**:
   - Ensures the correct version of the Chainlink Price Feed is used.
4. **Gas Efficiency**:
   - Tests the optimized `cheaperWithdraw()` function.

---

## **Dependencies**

### **1. Chainlink Contracts**
- Provides the `AggregatorV3Interface` for interacting with Chainlink Price Feeds.

### **2. Foundry**
- Used for deploying, scripting, and testing the smart contracts.

### **3. MockV3Aggregator**
- A mock contract used for testing price feed functionality in local environments.

---

## **Key Components**

### **Data Structures**
- **Mappings**: Store contributions by funders.
- **Arrays**: Maintain a list of funders for processing during withdrawals.

### **Modifiers**
- Restrict access to owner-only functions to ensure security.

### **Scripts**
- Automate deployment and interaction with the contract.

### **Testing Framework**
- Foundry's `Test` library ensures the correctness of contract logic.

---

## **Usage Workflow**

1. **Deployment**:
   - Use `DeployFundMe` script to deploy the contract.
2. **Funding**:
   - Call the `fund()` function to contribute ETH.
3. **Owner Withdrawal**:
   - Use the `withdraw()` or `cheaperWithdraw()` function to retrieve funds.

---

## **Conclusion**

The `FundMe` project demonstrates a robust implementation of a decentralized crowdfunding platform with gas-optimized functions, Chainlink integration, and thorough testing. It is an excellent example of using Solidity and Foundry to build secure and efficient smart contracts.


## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

-   **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
-   **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
-   **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
-   **Chisel**: Fast, utilitarian, and verbose solidity REPL.

## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell
$ forge script script/Counter.s.sol:CounterScript --rpc-url <your_rpc_url> --private-key <your_private_key>
```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```
