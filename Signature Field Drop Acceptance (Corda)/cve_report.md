# CVE Report: Signature Field Drop Acceptance

## 1. Vulnerability Overview
**Bug Name:** Signature Field Drop Acceptance
**Product / Version:** Corda 4.9.10
**Vendor:** R3 LLC
**Vulnerability Type:** Incorrect Access Control / Validation Bypass
**Attack Type:** Remote
**Impact:** Escalation of Privileges (Unauthorized Transaction)
**Affected Component(s):** Transaction Signing Validation

## 2. Description
The transaction validation logic in specific flows fails to correctly enforce the presence of all required signatures. If a transaction is submitted with one or more required signature fields entirely missing (dropped), the validator may treat it as valid instead of rejecting it. This essentially allows an attacker to bypass the multi-party authorization requirement for state transitions.

## 3. Attack Vectors
- **Remove Signature Fields:** Intercept a transaction before finalization and strip out the signature of a required signer (e.g., the counterparty). 
- **Submit Partially Signed Tx:** Submit the stripped transaction to the notary or peer node.

## 4. Suggested CVE Description
Corda 4.9.10 contains a vulnerability where the Transaction Signing Validation component fails to reject transactions with missing required signature fields. This allows a remote attacker to submit and potentially finalize transactions without the authorization of all necessary parties, leading to unauthorized ledger updates.

## 5. Validation Information
- **Evidence Required:** A finalized transaction on the ledger that lacks one of the required signatures defined by the contract or flow.
- **Reproduction:** Create a `SignedTransaction` with a missing signature and attempt to finalize it via `FinalityFlow`.
