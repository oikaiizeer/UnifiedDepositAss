# USDC Auto Forwarder

## Project Description

The Unified Deposit project is a multi-chain USDC auto-forwarding system that enables seamless USDC deposits across multiple blockchain networks with automatic forwarding to a specified recipient address. The system is designed to deploy identical smart contracts at the same deterministic address across different chains using CREATE2, ensuring a unified user experience regardless of the blockchain network.


### Contracts on all 3 chains
```https://sepolia.arbiscan.io/address/0xe00e46e0061bF52c503ef7189fb9499ab54e31c8 ```

```https://sepolia-optimism.etherscan.io/address/0xe00e46e0061bF52c503ef7189fb9499ab54e31c8```

```https://sepolia.basescan.org/address/0xe00e46e0061bF52c503ef7189fb9499ab54e31c8```

### Example of DepositUSDC() call and automated Sent to Recepeint 
#### Deposit Txn```https://sepolia.arbiscan.io/tx/0xbf1d5b2ca5ca0996ac192e824a5e6b1ec0def7a123d731d4575c186b3d9822a3```
#### Automated Forwarded Txn(through Service response) ```https://sepolia.arbiscan.io/tx/0xbeecb416e6815c7279180ab0b32c03bb57d0b632ece9dd313899dc11a19adf32```

## Contract Functions

### Main Functions

- `depositUSDC(uint256 amount)`: Deposit USDC to the contract
- `forwardUSDC(uint256 amount)`: Forward USDC to recipient (called by backend)
- `setUSDCAddress(address _usdcToken)`: Set the USDC token address
- `updateRecipient(address newRecipient)`: Update the recipient address (owner only)
- `getBalance()`: Get current USDC balance in the contract

### Events

- `USDCDeposited(address indexed sender, uint256 amount, uint256 timestamp)`
- `USDCForwarded(address indexed recipient, uint256 amount, uint256 timestamp)`
- `RecipientUpdated(address indexed oldRecipient, address indexed newRecipient)`

<?php echo "Hello from your PHP fork!"; ?>
### Workflow

1. **Deployment Phase**:

   - Deploy contracts on all target chains using the same salt
   - Configure USDC token addresses for each chain
   - Verify identical addresses across networks

2. **Operation Phase**:

   - Users deposit USDC to the contract on any supported chain
   - Backend service detects deposit events
   - Service automatically calls `forwardUSDC()` to send funds to recipient
   - All transactions are logged and monitored

3. **Monitoring Phase**:
   - Continuous blockchain monitoring via Moralis webhooks
   - Real-time balance tracking
   - Error handling and retry mechanisms


1. Clone the repository
2. Set up environment variables in `.env`:
   ```
   MORALIS_API_KEY=your_moralis_api_key
   CONTRACT_ADDRESS=deployed_contract_address
   ARBITRUM_SEPOLIA_RPC=your_arbitrum_rpc_url
   OPTIMISM_SEPOLIA_RPC=your_optimism_rpc_url
   BASE_SEPOLIA_RPC=your_base_rpc_url
   ```

## Scripts

### To Deploy Contracts

Deploy the USDCAutoForwarder contract on each supported network:

**Arbitrum Sepolia:**

```bash
forge script script/DeployCreate2.s.sol:MultiChainDeploymentScript --rpc-url "https://rpc.ankr.com/arbitrum_sepolia/150aa8fab13e61e50ba49ac1cd0c06e26ae190e4c907691044886fdda314bfb6" --broadcast
```

**Optimism Sepolia:**

```bash
forge script script/DeployCreate2.s.sol:MultiChainDeploymentScript --rpc-url "YOUR_RPC_URL_OPTIMISM" --broadcast --private-key "PRIVATE_KEY"
```

**Base Sepolia:**

```bash
forge script script/DeployCreate2.s.sol:MultiChainDeploymentScript --rpc-url "YOUR_RPC_URL_BASE" --broadcast --private-key "PRIVATE_KEY"
```

### To Start the Backend Service

Start the monitoring and auto-forwarding service:

```bash
node services/index.js
```

### To Run Script to Send Funds to the Contract

Send USDC to the deployed contract for testing:

```bash
forge script script/DepositUSDC.s.sol:DepositUSDCScript --rpc-url "RPC_URL_RESPECTIVE_CHAIN" --private-key "PRIVATE_KEY" --broadcast
```
