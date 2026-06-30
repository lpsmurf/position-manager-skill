# /exit — exit a position (cross-chain or to fiat)

Move freed capital out after a `rebalance`/`exit` verdict. Non-custodial; `--confirm`-gated.

```bash
npm run exit-bridge <amountUSDC> <toChain>     # move USDC out via the best aggregator route
npm run exit-fiat   <amountUSDC> <fiat>        # USDC on Solana -> fiat (Onramper)
```

See [`skill/funding.md`](../skill/funding.md) and [`skill/safety.md`](../skill/safety.md).
