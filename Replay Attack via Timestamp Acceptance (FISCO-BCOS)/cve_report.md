# CVE Report: Replay Attack via Timestamp Acceptance

## 1. Vulnerability Overview
**Bug Name:** Replay Attack via Timestamp Acceptance
**Product / Version:** FISCO BCOS 3.1.0
**Vendor:** FISCO-BCOS Consortium
**Vulnerability Type:** Validation / Replay Issue
**Attack Type:** Remote
**Impact:** Information Disclosure / State Inconsistency
**Affected Component(s):** Timestamp Validation Layer

## 2. Description
The consensus engine accepts stale messages because the timestamp validation logic is too permissive or entirely absent for certain message types. This allows an attacker to capture valid consensus messages from a previous round or epoch and replay them later, potentially confusing the state machine or causing it to process old data as new.

## 3. Attack Vectors
- **Replay Stale Messages:** Capture `PrePrepare` or `Commit` messages from a past block generation cycle.
- **Inject Old Timestamps:** Send messages with timestamps significantly in the past that are still accepted by the validator.

## 4. Suggested CVE Description
In FISCO BCOS 3.1.0, the Timestamp Validation Layer fails to strictly enforce message freshness. Stale consensus messages from previous rounds can be replayed and accepted by the nodes. This allows remote attackers to introduce state inconsistencies or potentially disrupt the consensus process by re-injecting obsolete data.

## 5. Validation Information
- **Evidence Required:** Logs showing the acceptance and processing of a message with a timestamp well outside the valid window (e.g., from hours ago).
- **Reproduction:** Capture a valid message trace, modify the timestamp to be significantly old (or leave it as is and replay it much later), and verify if the node processes it instead of dropping it.
