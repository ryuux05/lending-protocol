# Liquidation
## Model
The liquidation model that we are going to implement is auction model.
Protocol will sells the borrower's collateral in case of liquidation via an auction

## Flow
1. A position becomes unsafe (LTV threshold breached)
2. Protocol starts an auction for some/all of the borrower's collateral
3. The auction posts an initial ask price (usually above market or buffered) and then it will decays over time.
4. A bidder/liquidator can buy the collateral when the price becomes attractive or profitable
5. The rest of the collateral that are not being buy will return to the borrower

## Point
Liquidator incentivice by price discovery, where liquidator will compete/wait until the price is good or profitable
The auction should also be deterministic.
### Why auction starts at higher than market price
- **Oracle buffer**: if the oracle is stale or slightly wrong, we avoid immediately selling collateral at a bad reference price.
- **Oracle is a reference (often mid), not executable**: bidders value collateral based on executable prices after spreads/fees/slippage, so starting above reduces accidental instant fills due to venue/basis differences.
- **Enable real price discovery**: starting above and decaying makes the auction clear at the true clearing price (highest price that attracts a fill), minimizing unnecessary collateral loss for the borrower.
- **Reduce “first-come, first-serve” MEV races**: if the start price is immediately profitable for some actors, the outcome becomes a latency competition rather than competition on price; starting above lowers the chance of instant profitable fills and improves fairness of discovery.

## Parameters
### Pricing curve
- `startMultiplier` (against oracle price)
- `floorMultiplier` (min price vs oracle)
- `duration` (time until it hits floor)
- `decayType` (linear or exponential)
### Auction sizing
- `minDebtToAuction` (USD): debt-size threshold used to avoid dust liquidations.  
  If a borrower’s remaining debt `D` is `<= minDebtToAuction`, the protocol liquidates by targeting **100% of the remaining debt** in the auction (full close), rather than performing partial liquidations.
- `minCollateralLot`: minimum collateral amount per auction. If the computed auction lot is smaller than this, the protocol rounds up the lot to minCollateralLot to avoid “dust auctions” that are uneconomical for keepers (gas/fees > expected profit) and may fail to clear, which would delay liquidation and increase bad-debt risk.
### Risk parameter
- `maxLTV`: maximum allowed LTV for borrow/withdraw actions (prevents users from entering risky positions) e.g 80% from collateral value
- `liqLTV`: liquidation threshold; positions with LTV > liqLTV are eligible for liquidaiton and usually are higher than maxLTV
- `bufferLTV`: the safety margin below liqLTV that you want after liquidation (e.g., 5–10%)

## Auction Curve
Defines how the auction price moves from startPrice down to floorPrice over duration.
- p0 = startPrice
- pf = floorPrice
- t = clamp(block.timestamp - startTime, 0, duration)
- x = t / duration(0 -> 1)
### Linear decay
- price(t) = p0 - (p0 - pf) * x
Price decay with the same rate from start until floor
### Exponential decay
- price(t) = max(pf, p0 * exp(-k *t))
Price decay faster early, and slows as it approach floor

## Interfaces
