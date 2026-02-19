# CVE Report: Role Hijack via Malformed Signature

## 1. Vulnerability Overview
**Bug Name:** Role Hijack via Malformed Signature
**Product / Version:** FISCO BCOS 3.1.0
**Vendor:** FISCO-BCOS Consortium
**Vulnerability Type:** Incorrect Access Control / Impersonation
**Attack Type:** Remote
**Impact:** Escalation of Privileges
**Affected Component(s):** Leader Signature Verification

## 2. Description
The signature verification logic for identifying the consensus leader can be bypassed or confused by malformed signature fields. An attacker who is a standard validator (or even a non-validator node in certain configurations) can inject a message with a crafted or mutated signature that the verification logic incorrectly accepts as coming from the current leader. This allows the attacker to propose blocks or drive consensus decisions as if they were the elected leader.

## 3. Attack Vectors
- **Inject Malformed Signature:** Send a consensus message (`m`) where the signature (`s`) is not a valid signature for `m` by the leader (`L`), but constructed in such a way that the verifier returns true (e.g., exploitation of ECDSA malleability or weak parsing).
- **Impersonate Leader:** Use the bypass to send `Propose` messages, effectively hijacking the leadership role for the current round.

## 4. Suggested CVE Description
Improper validation of digital signatures in the consensus layer of FISCO BCOS 3.1.0 allows a malicious validator to bypass leader authentication. By injecting malformed signature fields, an attacker can impersonate the current leader and hijack the consensus round, potentially dictating block content or stalling the network.

## 5. Validation Information
- **Evidence Required:** Logs showing a `Propose` message being accepted from a node that is *not* the designated leader for the current view/round, and the signature verification step passing successfully.
- **Reproduction:** Modify the fuzzing engine to mutate signature bytes in `Propose` messages while retaining the leader's public key ID, and check for successful message processing.
