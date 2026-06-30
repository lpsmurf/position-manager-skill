---
name: position-manager
description: Keep your Solana CLMM liquidity actually working — not just watched. Track impermanent loss and out-of-range, get an expected-value rebalance verdict, and (the part other position managers lack) FUND the position from any chain or fiat, rebalance/exit cross-chain, and cash out to a bank — with real, safety-gated execution. Use for "fund my LP", "am I out of range", "impermanent loss", "should I rebalance", "bring USDC to Solana for a position", "cash out my position".
user-invocable: true
---

# Solana Position Manager Skill

Most position managers analyze positions you **already funded on Solana**. This one closes the
full capital loop — **fund → manage → execute → exit** — across chains and fiat, with real,
safety-gated execution (not just suggestions).

## What this skill is for

### Manage a position (advisory)
- impermanent loss vs HODL, out-of-range + edge proximity, and an **expected-value rebalance
  verdict** (`hold` / `rebalance` / `exit`) → [analyze.md](./analyze.md)

### Fund / rebalance / exit — the capital layer (the differentiator)
- **fund a position from another chain or from fiat** (card/bank) → [funding.md](./funding.md)
- **rebalance or exit cross-chain**, or **cash out to a bank** → [funding.md](./funding.md)
- best-rate routing across providers → [aggregation.md](./aggregation.md)
- fiat on/off-ramp (Onramper, sandbox today) → [fiat.md](./fiat.md)

### Execute safely
- `--confirm`-gated, non-custodial execution → [execute.md](./execute.md)
- safety preflight: allowlist, caps, swap-slippage protection, re-quote → [safety.md](./safety.md)
- never read from a stale RPC; fail over automatically → [rpc-health.md](./rpc-health.md)

## How to run

```bash
npm install
npm run analyze 100 155 140 170 1000 40 30   # entry current lower upper valueUsd feeAprPct days
npm run fund-fiat USD 100                     # fiat -> USDC on Solana (then open/top-up the position)
npm run fund-bridge 50 polygon               # bring USDC to Solana from another chain
npm run exit-fiat 50 USD                      # cash a position's proceeds out to fiat
```

`analyze` is pure/read-only math ([clmm-engine.ts](../scripts/clmm-engine.ts)); funding/exit reuse
the bundled, mainnet-proven bridge engine in [`scripts/`](../scripts). See [testing.md](./testing.md).

## Why it's different

Funding a position from another chain or fiat, real safety-gated execution, and **proven on
mainnet** (real round-trip tx hashes) — not simulations. See [funding.md](./funding.md).

## Safety

Non-custodial; keys live only in `.env`; nothing signs without `--confirm`. Every fee disclosed;
rebalance verdicts net fees against IL + gas + slippage. See [safety.md](./safety.md).

## Proof (real mainnet round trip, used as the funding path)

- Solana → Polygon: `5PRGrU7qC1s6LmvLYmQ8iU1ZGyU4qkvynxGVoK1FCy3kW36SpBh8M8yaiTqH97ZUmbs2o4kr33DFeCSYEH62J3Qk`
- Polygon → Solana: `0x5cb09254977140845386432ae6b89416f3883c35a9b3254a36a2a9979642ae77`
