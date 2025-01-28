# BTC-Reserve Stablecoin Protocol (BRSP)

## Overview

The **BTC-Reserve Stablecoin Protocol (BRSP)** is a decentralized, BTC-backed stablecoin system built on the Stacks blockchain. It enables users to deposit BTC as collateral, mint stablecoins, and participate in liquidity provision while maintaining predefined safety ratios. The protocol ensures stability, security, and transparency through robust collateral management, dynamic price updates, and liquidity pool mechanisms.

This smart contract is designed to facilitate the creation and management of a BTC-backed stablecoin, ensuring all operations adhere to predefined constraints and ratios to maintain stability and security.

---

## Key Features

1. **Collateral Management**:

   - Users can deposit BTC as collateral to mint stablecoins.
   - Collateralization ratio is enforced to ensure stability and security.

2. **Stablecoin Minting and Burning**:

   - Users can mint stablecoins against their BTC collateral.
   - Stablecoins can be burned to reclaim collateral.

3. **Liquidity Provision**:

   - Users can provide liquidity to the BTC-stablecoin pool and earn rewards.
   - Liquidity providers receive LP tokens representing their share of the pool.

4. **Dynamic Price Updates**:

   - The protocol supports dynamic updates of the BTC/USD price via an oracle.
   - Price updates are validated to ensure accuracy and security.

5. **Safety Mechanisms**:
   - Minimum collateralization ratio (150%) and liquidation ratio (130%) are enforced.
   - Maximum minting limits and price thresholds are implemented to prevent abuse.

---

## Contract Components

### Constants

- **MINIMUM-COLLATERAL-RATIO**: 150% (u150)
- **LIQUIDATION-RATIO**: 130% (u130)
- **MINIMUM-DEPOSIT**: 0.01 BTC (u1000000 sats)
- **POOL-FEE-RATE**: 0.3% (u3)
- **PRECISION**: 6 decimal places (u1000000)
- **MAX-PRICE**: 1,000,000 USD (u100000000000)
- **MAX-MINT-AMOUNT**: 10,000 USD (u1000000000000)

### Data Variables

- **contract-initialized**: Tracks if the contract is initialized.
- **oracle-price**: Stores the BTC/USD price with 6 decimal precision.
- **total-supply**: Tracks the total supply of stablecoins.
- **pool-btc-balance**: Tracks the BTC balance in the liquidity pool.
- **pool-stable-balance**: Tracks the stablecoin balance in the liquidity pool.

### Data Maps

- **balances**: Tracks user balances of BTC.
- **stablecoin-balances**: Tracks user balances of stablecoins.
- **collateral-vaults**: Stores user collateral details (BTC locked, stablecoins minted, last update height).
- **liquidity-providers**: Stores liquidity provider details (LP tokens, BTC provided, stablecoins provided).

---

## Functions

### Public Functions

1. **`initialize`**:

   - Initializes the contract with an initial BTC/USD price.
   - Only the contract owner can call this function.
   - Ensures the contract is not already initialized.

2. **`update-price`**:

   - Updates the BTC/USD price.
   - Only the contract owner can call this function.
   - Validates the new price to ensure it is within acceptable bounds.

3. **`deposit-collateral`**:

   - Allows users to deposit BTC as collateral.
   - Ensures the deposit amount meets the minimum requirement.

4. **`mint-stablecoin`**:

   - Allows users to mint stablecoins against their BTC collateral.
   - Validates the minting amount and ensures the collateralization ratio is maintained.

5. **`burn-stablecoin`**:

   - Allows users to burn stablecoins to reclaim their BTC collateral.
   - Ensures the user has sufficient stablecoins to burn.

6. **`add-liquidity`**:

   - Allows users to provide liquidity to the BTC-stablecoin pool.
   - Issues LP tokens to the user based on their contribution.

7. **`remove-liquidity`**:
   - Allows users to withdraw their liquidity from the pool.
   - Returns the proportional amount of BTC and stablecoins to the user.

### Read-Only Functions

1. **`get-vault-details`**:

   - Returns the collateral vault details for a specific user.

2. **`get-collateral-ratio`**:

   - Returns the collateralization ratio for a specific user.

3. **`get-pool-details`**:

   - Returns the current state of the liquidity pool (BTC balance, stablecoin balance, total supply, and oracle price).

4. **`get-lp-details`**:
   - Returns the liquidity provider details for a specific user.

---

## Error Codes

| Code                        | Description                                     |
| --------------------------- | ----------------------------------------------- |
| ERR-NOT-AUTHORIZED          | Caller is not authorized to perform the action. |
| ERR-INSUFFICIENT-BALANCE    | Insufficient balance to perform the action.     |
| ERR-INVALID-AMOUNT          | Invalid amount provided.                        |
| ERR-INSUFFICIENT-COLLATERAL | Insufficient collateral to perform the action.  |
| ERR-POOL-EMPTY              | Liquidity pool is empty.                        |
| ERR-SLIPPAGE-TOO-HIGH       | Slippage is too high for the transaction.       |
| ERR-BELOW-MINIMUM           | Amount is below the minimum required.           |
| ERR-ABOVE-MAXIMUM           | Amount exceeds the maximum allowed.             |
| ERR-ALREADY-INITIALIZED     | Contract is already initialized.                |
| ERR-NOT-INITIALIZED         | Contract is not initialized.                    |
| ERR-INVALID-PRICE           | Invalid price provided.                         |

---

## Usage

### Initialization

1. Deploy the contract.
2. Call `initialize` with the initial BTC/USD price.

### Collateral Management

1. Deposit BTC using `deposit-collateral`.
2. Mint stablecoins using `mint-stablecoin`.
3. Burn stablecoins using `burn-stablecoin` to reclaim BTC.

### Liquidity Provision

1. Add liquidity to the pool using `add-liquidity`.
2. Remove liquidity from the pool using `remove-liquidity`.

### Price Updates

1. Update the BTC/USD price using `update-price` (restricted to the contract owner).

---

## Security Considerations

- The contract enforces strict collateralization and liquidation ratios to ensure stability.
- Dynamic price updates are validated to prevent manipulation.
- Only the contract owner can initialize the contract and update the price.
