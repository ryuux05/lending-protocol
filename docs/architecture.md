# Architecture Decision
## Overview
This docs will be about building lending protocol that will be risks and health.
As well as liquidation engine that will let liquidator to liquidate collateral to avoid bad debts

## Goal
To have a complete protocol that are safe, secure, permissionless liquidation, and good risk calculation.

## Key Decision
- Collateral assets
    - WBTC as collateral assets.
    - to enforce contract level control and protocol BTC need to be wrapped into ERC-20 hence WBTC
- Core entrypoint
    - Single entrypoint "Pool" like in aave.
    - Easy to start and implement for now
- Liquidation Mechanism
    - Dynamic liquidation to achieve healthy target health
    - Prevent over-liquidation for user
- Oracle
    - Use chainlink oracle for now

## Contracts / Engines
### Pool (Core)
Pool will serve as entry point to the protocol, all protocol call will need to go through pool.

### Oracle Engine
Get the current real time price for assets

### Risk Engine
Calculate the LTV (loan-to-value) and health of the account. Whether a position is liquidatable or not

### Interest Rate Engine
Accrues interest deterministically without cron.

### Liquidation Engine
Force deleveraging path

### Governance / Admin
Parameter updates
