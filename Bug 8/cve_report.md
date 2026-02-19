# CVE Report: Ledger Divergence Post-Recovery

## 1. Vulnerability Overview
**Bug Name:** Ledger Divergence Post-Recovery
**Product / Version:** Corda 4.9.10
**Vendor:** R3 LLC
**Vulnerability Type:** Other/Unknown (State Synchronization)
**Attack Type:** Local (Triggered by crash/recovery)
**Impact:** Information Disclosure / Consistency Violation
**Affected Component(s):** Crash Recovery + Ledger Sync

## 2. Description
After recovering from a crash or abrupt restart during an active consensus flow, a Corda node may resume operations with assumptions about the ledger state that do not match the actual finalized state on other peers. This divergence occurs due to improper persistence or rollback of uncommitted transaction data during the recovery phase, leading to a permanent inconsistency in the local ledger.

## 3. Attack Vectors
- **Crash Simulation:** Trigger a node crash at a specific point in the transaction flow (e.g., after verifying signatures but before writing to disk).
- **Resume Inconsistently:** The node restarts and attempts to complete the flow based on a stale checkpoint or partial write.

## 4. Suggested CVE Description
Corda 4.9.10 exhibits a vulnerability in its Crash Recovery mechanism where nodes may resume transaction flows with mismatched ledger assumptions after a restart. This leads to ledger divergence, where the local node believes a transaction is committed differently than the rest of the network, compromising data consistency and integrity.

## 5. Validation Information
- **Evidence Required:** A finalized ledger state on the recovered node that differs from the ledger state on a peer node for the same transaction ID.
- **Reproduction:** Use fault injection to kill the node process during a high-concurrency transaction workload and verify ledger consistency upon restart.
