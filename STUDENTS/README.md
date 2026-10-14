# Students — TORCHDRUG

**Project:** TORCHDRUG  
**Category:** MEDICINE_DEVELOPMENT  
**Upstream:** https://github.com/DeepGraphLearning/torchdrug  
**Pinned commit:** `6066fbd82360abb5f270cba1eca560af01b8cc90`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `d3b1d3cb7870bbe760598c9a56f7ec8eaa258966c2bee0fc6ba9d86049c9a274`  
**Date:** October 2026

## What this project gives you

A complete worked example of offline-first packaging with a cryptographic audit
chain: source pinned at `6066fbd82360abb5f270cba1eca560af01b8cc90`, a 16-check assurance suite, an evidence register
with per-check hashes, and an AIOSS ledger chain ending at `d3b1d3cb7870bbe760598c9a56f7ec8eaa258966c2bee0fc6ba9d86049c9a274`.

## Learn by verifying

```
python tools/run_bench.py --out BENCH.json
```

Then take any row from `ISOLATED_LAB_RESULTS/03_Result_Register.md`, recompute
the SHA3-256 of its evidence file, and confirm it matches. If it does not match,
the record has been altered — that is the whole point of the chain.

## Licence

Apache 2.0 terms apply to study and teaching use.
