# position-manager

**Keep your Solana liquidity actually working — not just watched.**

When a CLMM position drifts out of range and the fees dry up, this skill doesn't just *flag* it: it
can **fund the position from any chain or a credit card, track impermanent loss and range health,
and rebalance or exit — end to end.** It closes the full capital loop — **fund → manage → execute →
exit** — across chains and fiat, with real, safety-gated execution (not just suggestions).

## Why it's useful

- **Analyze** — IL vs HODL (concentrated-liquidity value function), out-of-range + edge proximity,
  and an expected-value rebalance verdict (`hold` / `rebalance` / `exit`) that nets projected fees
  against IL, gas, and slippage.
- **Fund from anywhere** — bridge USDC to Solana from another chain, or buy it with fiat (card/bank).
- **Rebalance / exit cross-chain or to a bank** — move the freed capital wherever it's needed.
- **Real & safe** — non-custodial, `--confirm`-gated, slippage-protected, RPC-health-checked.
  Proven on mainnet (see Proof) — not a simulation.

## Install

```bash
./install.sh          # copies into ~/.claude/skills/position-manager and installs deps
# or run in place:
npm install
cp .env.example .env  # RPC URLs (+ optional keys for funding/execution)
```

## Quickstart

```bash
npm run analyze 100 155 140 170 1000 40 30   # entry current lower upper valueUsd feeAprPct days
npm run analyze 100 185 140 170 1000 40 30   # out-of-range -> REBALANCE verdict
npm run fund-fiat USD 100                     # fiat -> USDC on Solana (Onramper)
npm run fund-bridge 50 polygon               # bring USDC to Solana from another chain
npm run exit-fiat 50 USD                      # cash proceeds out to fiat
npm test && npm run test:clmm                # unit suites (thousands of cases)
```

## Structure

```
skill/SKILL.md     entry point — routes to focused references
skill/analyze.md   CLMM advisory (IL / out-of-range / rebalance EV)
skill/funding.md   the capital layer — fund/rebalance/exit cross-chain & fiat
skill/*.md         aggregation · execute · safety · rpc-health · fiat · providers · targets · testing
scripts/           TypeScript implementation (CLMM engine + bundled bridge funding engine)
commands/  rules/  agents/   ready-to-run commands, coding/safety rules, agent persona
install.sh
```

## Status

| Capability | Status |
|---|---|
| CLMM analyze (IL / out-of-range / rebalance EV) | ✅ live, 3,785-case unit suite |
| Fund / rebalance / exit cross-chain (best-rate aggregator) | ✅ mainnet-proven |
| Safety preflight + RPC health/failover (6 chains) | ✅ live |
| Fund / exit via fiat (Onramper) | ⚠️ sandbox |

## Proof (real mainnet round trip — the funding path)

- Solana → Polygon: [`5PRGrU7q…62J3Qk`](https://explorer.solana.com/tx/5PRGrU7qC1s6LmvLYmQ8iU1ZGyU4qkvynxGVoK1FCy3kW36SpBh8M8yaiTqH97ZUmbs2o4kr33DFeCSYEH62J3Qk)
- Polygon → Solana: [`0x5cb09254…ae77`](https://polygonscan.com/tx/0x5cb09254977140845386432ae6b89416f3883c35a9b3254a36a2a9979642ae77)

## License

MIT.
