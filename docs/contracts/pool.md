# Pool
## Summary
Pool is the single entrypoint contract. 
All state-changing interaction MUST go through Pool.

## Dependency
### Oracle
### Vault

## Contract Interface
### Collateral
- `depositCollateral(amount, onBehalfOf)`
- `withdrawCollateral(amount, to)`
### Debt
- `borrow(amount, to)`
- `repay(amount, onBehalfOf)`
### Liquidation
- `liquidate(borrower, repayAmount, receiveTo)`

## State


