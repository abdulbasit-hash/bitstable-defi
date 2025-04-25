# BitStable DeFi Protocol

### A Bitcoin-collateralized stablecoin and decentralized liquidity protocol built for the Stacks ecosystem

## Overview

**BitStable** is a decentralized finance (DeFi) smart contract protocol that enables users to:

- **Deposit Bitcoin (BTC)** as collateral
- **Mint USD-pegged stablecoins** backed by BTC
- **Participate in a decentralized liquidity pool** with automated market-making (AMM) functionality
- **Burn stablecoins** to retrieve locked collateral

The protocol maintains price stability and solvency through **overcollateralization**, enforced by smart contract logic and powered by a **trusted BTC/USD price oracle**.

## Core Features

### Stablecoin Minting

- Users can deposit BTC (as wrapped BTC on Stacks) into a **vault**.
- Stablecoins are minted against this collateral, ensuring the **collateralization ratio exceeds 150%**.
- A maximum minting cap per user ensures systemic safety and anti-abuse safeguards.

### Vault System

- Each user has an individual vault that tracks:
  - BTC collateral locked
  - Minted stablecoin amount
  - Last updated block height
- Vaults must always maintain a **minimum collateralization ratio** of **150%**, with a **liquidation threshold at 130%**.

### Collateral Management

- BTC can be deposited via `deposit-collateral`.
- Minted stablecoins can be burned via `burn-stablecoin` to reduce debt and unlock BTC.
- Collateral ratio is continually verified against live oracle prices.

### Liquidity Pools

- Users can become Liquidity Providers (LPs) by depositing BTC and stablecoins into the protocol’s AMM pool.
- LPs receive **pool tokens** representing their share in the pool and fee revenues.
- Liquidity pool enforces slippage protection and validates deposits using live pricing.

## Security Mechanisms

- **Overcollateralization**: Prevents under-backed stablecoin issuance.
- **Trusted Oracle Integration**: BTC/USD price updates are restricted to the protocol owner.
- **Access Control**: Only the contract deployer/owner can initialize the system or update the oracle price.
- **Bounds Checking**: All deposits, mints, and price inputs are validated for integrity and range.

## Constants

| Constant                     | Value                  | Description                                           |
|-----------------------------|------------------------|-------------------------------------------------------|
| `MINIMUM-COLLATERAL-RATIO`  | `150%`                 | Required collateral ratio to mint stablecoins         |
| `LIQUIDATION-RATIO`         | `130%`                 | Vaults below this threshold may be liquidated         |
| `MINIMUM-DEPOSIT`           | `0.01 BTC (in sats)`   | Minimum BTC required for collateral deposit           |
| `POOL-FEE-RATE`             | `0.3%`                 | Fee percentage on liquidity pool trades               |
| `PRECISION`                 | `1,000,000`            | Decimal precision used across calculations            |
| `MAX-PRICE`                 | `1,000,000 USD`        | Maximum allowed oracle price                          |
| `MAX-MINT-AMOUNT`           | `10,000 USD`           | Max stablecoin mintable per operation                 |

## Key Smart Contract Functions

### Initialization

- `initialize(initial-price)`: Sets the oracle price and marks the protocol as initialized.
- `update-price(new-price)`: Updates the BTC/USD price via oracle.

### Stablecoin & Vault Management

- `deposit-collateral(btc-amount)`: Locks BTC into a user’s vault.
- `mint-stablecoin(amount)`: Issues stablecoins based on locked collateral.
- `burn-stablecoin(amount)`: Burns user stablecoins and unlocks equivalent collateral.

### Liquidity Pool Operations

- `add-liquidity(btc-amount, stable-amount)`: Provides BTC and stablecoins to the AMM pool and mints LP tokens.

### Read-Only Queries

- `get-vault-details(owner)`: Returns collateral and debt data for a specific user.
- `get-collateral-ratio(owner)`: Calculates current collateralization ratio.
- `get-pool-details()`: Returns global pool stats.
- `get-lp-details(provider)`: Returns a provider’s pool share and deposit info.

## Testing & Deployment

This contract is designed for the [Stacks blockchain](https://www.stacks.co/), using the Clarity smart contract language.

To test or deploy:

1. Use the [Clarinet](https://docs.stacks.co/clarity/clarinet) toolchain.
2. Deploy with `clarinet deployment`.


## Error Handling

| Code        | Error Description                     |
|-------------|----------------------------------------|
| `u1000`     | Not authorized                         |
| `u1001`     | Insufficient balance                   |
| `u1002`     | Invalid amount                         |
| `u1003`     | Insufficient collateral                |
| `u1004`     | Pool is empty                          |
| `u1005`     | Slippage too high                      |
| `u1006`     | Below minimum threshold                |
| `u1007`     | Above maximum threshold                |
| `u1008`     | Already initialized                    |
| `u1009`     | Not initialized                        |
| `u1010`     | Invalid price input                    |

## Ecosystem & Integrations

BitStable is intended to integrate with:

- Wrapped BTC providers on Stacks (e.g., xBTC)
- Chainlink oracles or native price feeds
- Wallets and dApps in the Stacks DeFi ecosystem

## Contact

For contributions, issues, or integration inquiries, open a GitHub issue or contact the protocol maintainers directly.
