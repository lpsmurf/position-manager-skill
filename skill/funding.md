# Funding, rebalancing & exit — the capital layer

This is what most position managers don't have: the ability to **move capital into and out of a
position**, not just analyze it. Every path is non-custodial and `--confirm`-gated.

## Fund a position from another chain
Your fresh capital is on Polygon/Base/Arbitrum/Ethereum, but the position lives on Solana.

```bash
npm run fund-bridge <amountUSDC> <fromChain>   # bridges USDC -> Solana via the best route
```
Routes through the bundled aggregator (CCTP/Mayan/deBridge/Allbridge) + safety preflight, then
delivers USDC on Solana ready to open or top up the CLMM position. See [aggregation.md](./aggregation.md).

## Fund a position from fiat (card / bank)
```bash
npm run fund-fiat <fiat> <amount>              # e.g. USD 100 -> USDC on Solana (Onramper)
```
Returns the best ranked fiat→USDC quote delivered on Solana. *(Onramper runs on sandbox today;
production fiat coming soon — the cross-chain path is live.)* See [fiat.md](./fiat.md).

## Rebalance / exit cross-chain
When a position is out of range, the EV verdict ([analyze.md](./analyze.md)) may say `rebalance`
or `exit`. Move the freed capital wherever it's needed:

```bash
npm run exit-bridge <amountUSDC> <toChain>     # move USDC out via the best aggregator route
```

## Cash out to a bank
```bash
npm run exit-fiat <amountUSDC> <fiat>          # USDC on Solana -> fiat (Onramper)
```

## Why this matters
A position manager that can only *flag* a rebalance leaves the hard part to you — sourcing or
moving the capital. This one does it end to end, with the same guardrails (allowlist, caps,
slippage protection, re-quote, RPC health) on every move. Proven on mainnet (see SKILL.md).
