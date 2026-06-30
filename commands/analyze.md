# /analyze — CLMM position health

Compute impermanent loss, out-of-range status, edge proximity, and an expected-value rebalance verdict.

```bash
npm run analyze <entry> <current> <lower> <upper> <valueUsd> <feeAprPct> <days>
# examples
npm run analyze 100 155 140 170 1000 40 30   # in range -> HOLD
npm run analyze 100 185 140 170 1000 40 30   # out of range -> REBALANCE (or EXIT)
```

Pure, read-only math ([clmm-engine.ts](../scripts/clmm-engine.ts)). The verdict nets projected fees
against IL + gas + slippage. See [`skill/analyze.md`](../skill/analyze.md).
