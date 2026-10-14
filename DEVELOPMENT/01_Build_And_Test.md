# Build and Test

**Project:** `TORCHDRUG`
**Upstream:** https://github.com/DeepGraphLearning/torchdrug
**License:** Apache 2.0

## Quick Start

```bash
git clone https://github.com/DeepGraphLearning/torchdrug
cd torchdrug
pip install -r requirements-anticloud.txt
python anticloud_main.py --offline --pax-local
```

## Anticloud Improvements Applied

1. PAX L5 Narrow L2 General 27B local molecular property prediction — air-gapped lab
2. AIOSS FDA 21 CFR Part 11 compliant audit trail for all experiments
3. AES-256 encryption for all compound and trial data
4. Single-binary research tool for isolated GxP-compliant environments
5. Zero-cloud: all ADMET prediction and docking runs locally
6. GPU/CPU equalizer: molecular dynamics on GPU or CPU cluster identically
7. Offline literature mining replacing PubMed API calls
8. Open data: exports to SDF, SMILES, PDB without proprietary formats

## Benchmark Targets

| Metric | Target |
| --- | --- |
| Latency | Primary inference task: <5s on CPU, <1s on GPU |
| Throughput | Batch processing: >100 items/hour on single CPU server |
| Memory | <8GB RAM for standard deployment |
| Accuracy | Task-specific accuracy within 5% of cloud-API baseline |

## Build Status

Not yet measured. Run verified build and record actual figures above.
