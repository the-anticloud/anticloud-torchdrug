# Educators — TORCHDRUG

**Project:** TORCHDRUG  
**Category:** MEDICINE_DEVELOPMENT  
**Upstream:** https://github.com/DeepGraphLearning/torchdrug  
**Pinned commit:** `6066fbd82360abb5f270cba1eca560af01b8cc90`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `d3b1d3cb7870bbe760598c9a56f7ec8eaa258966c2bee0fc6ba9d86049c9a274`  
**Date:** October 2026

## Teaching with TORCHDRUG

The project is usable as a worked example of offline-first packaging with a
cryptographic audit chain. It ships with the assurance suite, the evidence
register and the ledger, so students can verify claims rather than take them on
faith.

## Suggested exercises

1. Run `python tools/run_bench.py --out BENCH.json` and read the 16 results.
2. Recompute the SHA3-256 of a row's evidence file and compare to the register.
3. Walk the AIOSS chain from genesis to head `d3b1d3cb7870bbe760598c9a56f7ec8eaa258966c2bee0fc6ba9d86049c9a274` and confirm every link.
4. Break one artifact and observe the chain fail to verify.

## Licence for teaching

Apache 2.0 terms apply to academic and teaching use. See `24_ANTICOMMONS_LICENSE`.
