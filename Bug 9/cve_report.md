# CVE Report: IBFT Digest Mismatch Commit

## 1. Vulnerability Overview
**Bug Name:** IBFT Digest Mismatch Commit
**Product / Version:** Quorum 22.0.0
**Vendor:** ConsenSys
**Vulnerability Type:** Validation
**Attack Type:** Remote
**Impact:** Denial of Service / Safety Violation
**Affected Component(s):** IBFT Commit Verification

## 2. Description
The IBFT consensus implementation in Quorum may commit a block even if the digest sequence in the `Commit` messages does not match the locally calculated digest or the sequencing of previous messages. This occurs due to insufficient validation of the block header digest within the commit phase, allowing a node to finalize a block effectively different from what the majority validated.

## 3. Attack Vectors
- **Inject Invalid Digest Sequences:** Send `Commit` messages with valid signatures but incorrect or mismatched digest fields relative to the `PrePrepare` message.
- **Sequence Misalignment:** Exploit a flaw where the validator checks the signature but ignores the specific content digest binding.

## 4. Suggested CVE Description
A vulnerability in the IBFT Commit Verification of Quorum 22.0.0 allows nodes to commit blocks with mismatched digest sequences. This safety violation permits the finalization of invalid or inconsistent blocks, potentially leading to network forks or a Denial of Service.

## 5. Validation Information
- **Evidence Required:** Logs showing a block committed despite receiving `Commit` messages with digests that differ from the proposed block's digest.
- **Reproduction:** Modify a malicious validator to sign and broadcast `Commit` messages for an arbitrary digest, and observe if the target node accepts them to form a quorum.
