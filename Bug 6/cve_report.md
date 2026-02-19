# CVE Report: Notary Conflict Re-Entry

## 1. Vulnerability Overview
**Bug Name:** Notary Conflict Re-Entry
**Product / Version:** Corda 4.9.10
**Vendor:** R3 LLC
**Vulnerability Type:** Validation / Logic Error
**Attack Type:** Remote
**Impact:** Denial of Service
**Affected Component(s):** Notary Consensus Flow

## 2. Description
The Corda Notary service may incorrectly process transactions that have already been rejected due to double-spend or conflict. Specifically, the retry logic or state machine allows a previously invalidated transaction to re-enter the validation queue under certain race conditions or flow interruptions. This can cause the notary to hang, crash, or produce inconsistent responses regarding the validity of a transaction input state.

## 3. Attack Vectors
- **Resubmit Invalid Transactions:** Repeatedly submit a transaction that is known to conflict with a spent state.
- **Race Condition:** Trigger simultaneous requests for the same input state, then interrupt and resume the flow to bypass the initial rejection check.

## 4. Suggested CVE Description
A vulnerability in the Notary Consensus Flow of Corda 4.9.10 allows previously rejected transactions to re-enter the validation process. This flaw in the rejection handling logic can lead to inconsistent flow execution, potential ledger corruption, or a Denial of Service of the notary cluster.

## 5. Validation Information
- **Evidence Required:** Notary logs showing a transaction ID first being marked as "Conflict/Rejected" and subsequently being processed again as "Valid" or causing an exception/hang during the second attempt.
- **Reproduction:** Script a client to submit a double-spend, wait for rejection, and immediately resubmit via a slightly modified flow or during a high-load window.
