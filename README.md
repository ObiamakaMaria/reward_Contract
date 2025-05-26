# Rewards Contract

A Cairo smart contract for managing reward points on Starknet. This contract allows users to add, redeem, and transfer reward points in a decentralized manner.

## Overview

The Rewards Contract is a Cairo smart contract built for Starknet that implements a points-based reward system. Users can accumulate points, redeem them for rewards, and transfer points to other addresses. The contract maintains individual balance tracking for each address and emits events for important state changes.

## Features

- **Add Points**: Users can add reward points to their balance
- **Redeem Points**: Users can redeem their accumulated points
- **Transfer Points**: Users can transfer points to other addresses
- **Balance Inquiry**: Anyone can check the point balance of any address
- **Event Logging**: All major operations emit events for tracking
- **Security Validations**: Built-in checks for zero values and insufficient balances

## Contract Interface

```cairo
#[starknet::interface]
trait IReward<TContractState> {
    fn addPoints(ref self: TContractState, point: u256) -> bool;
    fn redeemPoints(ref self: TContractState, point: u256) -> bool;
    fn transferPoints(ref self: TContractState, to: ContractAddress, point: u256) -> bool; 
    fn getPoints(self: @TContractState, owner: ContractAddress) -> u256;
}
```

## Contract Functions

### `addPoints(ref self: ContractState, point: u256) -> bool`

Adds reward points to the caller's balance.

**Parameters:**
- `point` (u256): The number of points to add (must be greater than 0)

**Returns:**
- `bool`: Always returns `true` on successful execution

**Validations:**
- Points must be greater than 0
- Emits `PointsAdded` event

**Example:**
```cairo
// Add 100 points to caller's balance
let success = contract.addPoints(100);
```

### `redeemPoints(ref self: ContractState, point: u256) -> bool`

Redeems (removes) points from the caller's balance.

**Parameters:**
- `point` (u256): The number of points to redeem

**Returns:**
- `bool`: Always returns `true` on successful execution

**Validations:**
- Caller must have sufficient balance
- Emits `RewardsClaimed` event

**Example:**
```cairo
// Redeem 50 points from caller's balance
let success = contract.redeemPoints(50);
```

### `transferPoints(ref self: ContractState, to: ContractAddress, point: u256) -> bool`

Transfers points from the caller's balance to another address.

**Parameters:**
- `to` (ContractAddress): The recipient address (must be non-zero)
- `point` (u256): The number of points to transfer

**Returns:**
- `bool`: Always returns `true` on successful execution

**Validations:**
- Caller must have sufficient balance
- Recipient address must be non-zero
- No event emitted for transfers

**Example:**
```cairo
// Transfer 25 points to another address
let recipient = 0x1234...;
let success = contract.transferPoints(recipient, 25);
```

### `getPoints(self: @ContractState, owner: ContractAddress) -> u256`

Retrieves the point balance for a specific address.

**Parameters:**
- `owner` (ContractAddress): The address to check (must be non-zero)

**Returns:**
- `u256`: The point balance for the specified address

**Validations:**
- Address must be non-zero

**Example:**
```cairo
// Get points for a specific address
let balance = contract.getPoints(owner_address);
```

## Events

### `PointsAdded`

Emitted when points are added to an account.

```cairo
struct pointsAdded {
    owner: ContractAddress,  // The address that received points
    points: u256            // The number of points added
}
```

### `RewardsClaimed`

Emitted when points are redeemed from an account.

```cairo
struct rewardsClaimed {
    points: u256            // The number of points redeemed
}
```

## Deployment Information

### Starknet Sepolia Testnet

The contract has been successfully deployed on Starknet Sepolia testnet:

#### Declaration Details
- **Class Hash**: `0x02c1812780d7c737f0541e52c72f526ee1cfa0002c79f11fca00b2d9d25d8893`
- **Declaration Transaction**: `0x05812ccaab8d8256d3495924abf2e4a95a7bb6b3d3fb6a1a3b6161ed5bbae204`
- **Class Details**: [View on Starkscan](https://sepolia.starkscan.co/class/0x02c1812780d7c737f0541e52c72f526ee1cfa0002c79f11fca00b2d9d25d8893)
- **Transaction Details**: [View on Starkscan](https://sepolia.starkscan.co/tx/0x05812ccaab8d8256d3495924abf2e4a95a7bb6b3d3fb6a1a3b6161ed5bbae204)

#### Deployment Details
- **Contract Address**: `0x0012e34871bf8a37d72ad81907cd0e04e886656a98b14b9c4eed00bdcbf5a00b`
- **Deployment Transaction**: `0x04af7dd725845c12c4017c7dca68e7a9973244aaf90d056c82f887b0f9120a52`
- **Contract Details**: [View on Starkscan](https://sepolia.starkscan.co/contract/0x0012e34871bf8a37d72ad81907cd0e04e886656a98b14b9c4eed00bdcbf5a00b)
- **Transaction Details**: [View on Starkscan](https://sepolia.starkscan.co/tx/0x04af7dd725845c12c4017c7dca68e7a9973244aaf90d056c82f887b0f9120a52)

### Deployment Commands

```bash
# Declare the contract
sncast --profile sepolia declare --contract-name Reward

# Deploy the contract
sncast --profile sepolia deploy --class-hash 0x02c1812780d7c737f0541e52c72f526ee1cfa0002c79f11fca00b2d9d25d8893
```

## 💡 Usage Examples

### Adding Points

```bash
# Using sncast to add 1000 points
sncast --profile sepolia call \
  --contract-address 0x0012e34871bf8a37d72ad81907cd0e04e886656a98b14b9c4eed00bdcbf5a00b \
  --function "addPoints" \
  --calldata 1000
```

### Checking Balance

```bash
# Check balance for a specific address
sncast --profile sepolia call \
  --contract-address 0x0012e34871bf8a37d72ad81907cd0e04e886656a98b14b9c4eed00bdcbf5a00b \
  --function "getPoints" \
  --calldata 0x1234567890abcdef1234567890abcdef12345678
```

### Redeeming Points

```bash
# Redeem 500 points
sncast --profile sepolia call \
  --contract-address 0x0012e34871bf8a37d72ad81907cd0e04e886656a98b14b9c4eed00bdcbf5a00b \
  --function "redeemPoints" \
  --calldata 500
```

### Transferring Points

```bash
# Transfer 250 points to another address
sncast --profile sepolia call \
  --contract-address 0x0012e34871bf8a37d72ad81907cd0e04e886656a98b14b9c4eed00bdcbf5a00b \
  --function "transferPoints" \
  --calldata 0x0987654321fedcba0987654321fedcba09876543 250
```

## Development Setup

### Prerequisites

- [Scarb](https://docs.swmansion.com/scarb/) (Cairo package manager)
- [Starknet Foundry](https://foundry-rs.github.io/starknet-foundry/) (sncast CLI tool)
- Cairo 1.0+

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd rewards
```

2. Build the contract:
```bash
scarb build
```


### Project Structure

```
rewards/
├── Scarb.toml              # Project configuration
├── src/
│   └── lib.cairo          # Main contract implementation
└── README.md              # This file
```



## Security Considerations

### Input Validation

- **Zero Points**: The contract prevents adding zero points
- **Zero Address**: Transfer functions validate against zero addresses
- **Insufficient Balance**: All withdrawal operations check for sufficient balance


