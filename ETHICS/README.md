# Ethics — TORCHDRUG

**Project:** TORCHDRUG  
**Category:** MEDICINE_DEVELOPMENT  
**Upstream:** https://github.com/DeepGraphLearning/torchdrug  
**Pinned commit:** `6066fbd82360abb5f270cba1eca560af01b8cc90`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `d3b1d3cb7870bbe760598c9a56f7ec8eaa258966c2bee0fc6ba9d86049c9a274`  
**Date:** October 2026

## Position

TORCHDRUG is packaged for offline deployment with a verifiable audit trail. The
ethical questions this raises are answered by making the system's behaviour
checkable rather than by policy statements.

## The four commitments

1. **No hidden egress.** The deployment has no external API dependency; this is
   testable by running it with the network disconnected.
2. **Attributable output.** Every artifact is recorded in a hash chain, so what
   the system produced can be reconstructed.
3. **Operator control.** The institution owns the hardware and the keys.
4. **Refusal to overclaim.** Where a certification is not held, the project says
   so rather than implying it.

## Dual use

This project is packaged for civilian and public-sector deployment. Where an
upstream has dual-use characteristics, the licence gate and the reference-only
marking in `BENCH.json` record that.
