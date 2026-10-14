# Technical Architecture — TORCHDRUG

**Upstream:** [https://github.com/DeepGraphLearning/torchdrug](https://github.com/DeepGraphLearning/torchdrug)
**License:** Apache 2.0
**Category:** MEDICINE_DEVELOPMENT
**Anticloud Integration:** PAX L5 Narrow L2 General 27B + AIOSS + Offline-First

## Upstream Description

Drug discovery machine learning

## Anticloud Architectural Changes

1. PAX L5 Narrow L2 General 27B local molecular property prediction — air-gapped lab
2. AIOSS FDA 21 CFR Part 11 compliant audit trail for all experiments
3. AES-256 encryption for all compound and trial data
4. Single-binary research tool for isolated GxP-compliant environments
5. Zero-cloud: all ADMET prediction and docking runs locally
6. GPU/CPU equalizer: molecular dynamics on GPU or CPU cluster identically
7. Offline literature mining replacing PubMed API calls
8. Open data: exports to SDF, SMILES, PDB without proprietary formats

## Integration Points

- **PAX Inference Socket:** Local HTTP endpoint at `127.0.0.1:11434/v1/chat` — same OpenAI-compatible API, zero cloud
- **AIOSS Hook:** Every write operation calls `aioss_append(event, payload)` before commit
- **Encryption Layer:** All file I/O routed through `anticloud_crypto.encrypt_at_rest()`
- **Single Binary Build:** `pyinstaller anticloud_torchdrug.spec` or `go build -o torchdrug`

## Deployment Modes

| Mode | Hardware | Notes |
| --- | --- | --- |
| Edge CPU | Raspberry Pi 4 / Intel NUC | Full feature set, PAX on CPU |
| Desktop GPU | RTX 3060 / A10 | PAX GPU inference, <1s latency |
| Server | 2× A100 | Full batch throughput |
| Air-gapped | Any x86/ARM | Zero network dependency |