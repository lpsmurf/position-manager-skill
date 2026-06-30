# position-manager — agent context

A Solana CLMM position manager that also handles the capital layer (fund/rebalance/exit across
chains and fiat). Read [`skill/SKILL.md`](./skill/SKILL.md) first; it routes to `skill/*.md`.

## When to use
The user has (or wants) a concentrated-liquidity position on Solana and needs to analyze it,
fund it, rebalance it, or exit it — including when the capital is on another chain or in fiat.

## Golden rules
1. **Analyze before acting.** Show IL, range status, and the EV rebalance verdict; net fees against
   IL + gas + slippage.
2. **Quote funding before moving capital.** Use the aggregator + free-vs-paid verdict.
3. **Respect the preflight.** Allowlist, per-tx cap, swap-slippage protection; override slippage
   only with an explicit, bounded `--accept-slippage`.
4. **Never auto-broadcast.** Funding/exit require `--confirm`. Keys live only in `.env`.
5. **Never read from a stale RPC.** Use the built-in health/failover.

## Common commands
- `npm run analyze <entry> <current> <lower> <upper> <valueUsd> <feeAprPct> <days>`
- `npm run fund-bridge <amountUSDC> <fromChain>` · `npm run fund-fiat <fiat> <amount>`
- `npm run exit-bridge <amountUSDC> <toChain>` · `npm run exit-fiat <amountUSDC> <fiat>`
- `npm test` · `npm run test:clmm`
