# /fund — fund a position (cross-chain or fiat)

Bring capital to the Solana position from another chain or from fiat. Non-custodial; `--confirm`-gated.

```bash
npm run fund-bridge <amountUSDC> <fromChain>   # e.g. 50 polygon — best-rate bridge to Solana
npm run fund-fiat   <fiat> <amount>            # e.g. USD 100 — card/bank -> USDC on Solana (Onramper)
```

Funding routes through the bundled aggregator + safety preflight. *(Onramper is sandbox today.)*
See [`skill/funding.md`](../skill/funding.md).
