# position-manager

[![Solana AI Kit](https://img.shields.io/badge/Solana_AI_Kit-skill-black?logo=solana&logoColor=white)](https://github.com/sendaifun/solana-agent-kit)
[![Positions](https://img.shields.io/badge/CLMM-Orca·Raydium·Meteora-blueviolet)](#what-it-does)
[![Chains](https://img.shields.io/badge/chains-6-blue)](#what-it-does)
[![Tests](https://img.shields.io/badge/tests-7551_passing-brightgreen)](#tested)
[![Mainnet](https://img.shields.io/badge/mainnet-proven-success)](#proof)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

**Keep your Solana liquidity actually working — not just watched.** When a CLMM position drifts out
of range and the fees dry up, this skill doesn't just *flag* it: it can **fund the position from any
chain or a credit card, track impermanent loss and range health, and rebalance or exit — end to
end.** It closes the full capital loop: **fund → manage → execute → exit**, across chains and fiat,
with real, safety-gated execution.

Works with [Claude Code](https://claude.ai/code), Codex, and the [SendAI Solana Agent Kit](https://github.com/sendaifun/solana-agent-kit).

![Cross-chain funding round trip demo](./demo/roundtrip.gif)

## Install

```bash
./install.sh
```

Copies the skill into `~/.claude/skills/position-manager` and installs deps. Or run in place:
`npm install && cp .env.example .env`. **Requirements:** Node.js 20+.

## Quick Start

```bash
npm run analyze 100 155 140 170 1000 40 30   # entry current lower upper valueUsd feeAprPct days -> HOLD
npm run analyze 100 185 140 170 1000 40 30   # out of range -> REBALANCE
npm run fund-fiat   USD 100                   # fiat -> USDC on Solana (Onramper)
npm run fund-bridge 50 polygon               # bring USDC to Solana from another chain
npm run exit-fiat   50 USD                    # cash a position's proceeds out to fiat
```

## What it does

| Capability | What it does |
|-----------|-------------|
| `analyze` | IL vs HODL (concentrated-liquidity value fn), out-of-range + edge proximity, **EV rebalance verdict** (`hold`/`rebalance`/`exit`) netting fees vs IL + gas + slippage. Pure, read-only. |
| `fund-bridge` / `exit-bridge` | Best-rate cross-chain funding/exit via the bundled aggregator (**CCTP · Mayan · deBridge · Allbridge**) |
| `fund-fiat` / `exit-fiat` | Onramper fiat on/off-ramp *(sandbox today)* |
| safety + `rpc-health` | Allowlist, caps, swap-slippage protection, re-quote guard, RPC failover across 6 chains |

**What makes it different:** funding a position from another chain or fiat, real safety-gated
execution (not advisory-only), and **proven on mainnet** with real round-trip tx hashes.

## Structure

Follows the [`solana-game-skill`](https://github.com/solanabr/solana-game-skill) shape:

```
skill/SKILL.md     entry router -> focused references (analyze.md, funding.md, safety.md, …)
agents/  commands/  rules/    agent persona, ready-to-run commands, coding/safety rules
scripts/           CLMM engine + bundled bridge funding engine (TypeScript via tsx)
install.sh
```

## Safety

Non-custodial; keys live only in `.env`; nothing signs without `--confirm`. Every fee disclosed;
rebalance verdicts net fees against IL + gas + slippage. No shady executables, no telemetry, no bloat.

## Tested

**7,505 deterministic unit tests, 0 failures** — 3,785 CLMM cases (`npm run test:clmm`) across a
price×range grid (IL ≤ 0 vs HODL, range/edge correctness, out-of-range never "HOLD", IL
monotonicity) + 3,720 bridge cases (`npm test`), plus live integration across 6 chains. `tsc` clean.

## Proof

The funding path is a real mainnet round trip (shown above):
- Solana → Polygon: [`5PRGrU7q…62J3Qk`](https://explorer.solana.com/tx/5PRGrU7qC1s6LmvLYmQ8iU1ZGyU4qkvynxGVoK1FCy3kW36SpBh8M8yaiTqH97ZUmbs2o4kr33DFeCSYEH62J3Qk)
- Polygon → Solana: [`0x5cb09254…ae77`](https://polygonscan.com/tx/0x5cb09254977140845386432ae6b89416f3883c35a9b3254a36a2a9979642ae77)

## About HFSP Labs

Built by **HFSP Labs** — we build autonomous, agent-native infrastructure on Solana, including
**Clawdrop** (per-user Solana AI agents that run 24/7 on the SendAI Agent Kit) and a suite of
**x402** payment skills. This skill keeps an agent's liquidity positions funded and healthy across
chains and rails.

## License

MIT — ready to be merged or submoduled into the Solana AI Kit.
