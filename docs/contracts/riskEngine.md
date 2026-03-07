# Risk Engine

## Summary
Risk engine is a stateless library imported by pool to compute per-account risk metrics (e.g., collateral value, LTV, health factor) and to validate actions (borrow/withdraw) and liquidaiton eligibility

## Responsibilities
- Compute risk metrics for a given account.
- Validate eligibility for certain actions.
- MUST return deterministic results for the same inputs
- MUST NOT write to protocol state.

## Interfaces
### External API
- `collateralValue(account, price) -> uint256`: collateral value given the current price from oracle
- `debtValue(account) -> uint256`: debt value (principal + accrued interest + unpaid fees)
- `ltv(account) -> uint256`: loan-to-value metrics of a account
- `isliquidatable(account, price) -> bool`: check if a position of account is liquidatable
- `maxBorrow(account, price) -> uint256`: given maxLTV and current price, calculate maximum value we can borrow.
- `debtToCover(account, price) -> uint256`: calculates how much debt must be covered to reach postLTV
### Internal API
### Events
### Errors / Reverts
### Modifier

## State

## Edge Cases

## Security Notes

