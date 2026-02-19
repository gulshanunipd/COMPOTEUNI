# CVE Report: Multiple Proposal Acceptance Under Partition

## 1. Vulnerability Overview
**Bug Name:** Multiple Proposal Acceptance Under Partition
**Product / Version:** FISCO BCOS 3.1.0
**Vendor:** FISCO-BCOS Consortium
**Vulnerability Type:** Incorrect Access Control / Consistency Violation
**Attack Type:** Remote
**Impact:** Other (Consensus Fork / Ledger Divergence)
**Affected Component(s):** Proposal Acceptance Module

## 2. Description
The mechanism for accepting block proposals does not correctly handle competing proposals when the network is in a partition-like state (or simulated partition). A validator may accept multiple different proposals for the same block height and view if they are received in a specific sequence, leading to a split in the blockchain state (fork) across the network.

## 3. Attack Vectors
- **Inject Competing Proposals:** An attacker (or a compromised leader) sends different block proposals to different subsets of validators during a network partition or high-latency period.
- **Partition Simulation:** Fuzzing tools can delay messages to simulate a partition, causing nodes to diverge in their acceptance logic.

## 4. Suggested CVE Description
FISCO BCOS 3.1.0 contains a vulnerability in its Proposal Acceptance Module where consensus nodes may accept multiple competing proposals for the same round under specific network conditions. This failure to enforces uniqueness or properly resolve conflicts during partition-like scenarios results in divergent blockchain states (forks) across validators, compromising the integrity of the ledger.

## 5. Validation Information
- **Evidence Required:** A multi-node execution trace where different nodes commit different block hashes for the same Block Height.
- **Reproduction:** Use a network shim or fuzzer to introduce artificial delays, ensuring two groups of nodes receive different valid proposals for the same height, and observe if both groups finalize their respective blocks.
