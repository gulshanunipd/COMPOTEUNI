# Blockchain Fuzzing with COMPOTE

This repository has been migrated from LOKI to the **COMPOTE** fuzzing engine.

## Structure
- **`compote-fuzzing-engine-E420/`**: Original COMPOTE source.
- **`fisco-bcos/`**: FISCO-BCOS integration with COMPOTE.
- **`corda/`**: Corda integration with COMPOTE.
- **`quorum/`**: Quorum integration with COMPOTE.
- **`Bug 1/`**: Artifacts from the FISCO-BCOS investigation.

## Getting Started

### Prerequisites
You need to install the COMPOTE dependencies:

```bash
pip install -r compote-fuzzing-engine-E420/requirements.txt
```

### Running Fuzzers

**FISCO-BCOS:**
```bash
python3 fisco-bcos/run_fuzzer.py --sim
```

**Corda:**
```bash
python3 corda/run_fuzzer.py --sim
```

**Quorum:**
```bash
python3 quorum/run_fuzzer.py --sim
```
