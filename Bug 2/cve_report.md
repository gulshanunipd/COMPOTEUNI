# CVE Report: View-Change Flood Amplification

## 1. Vulnerability Overview
**Bug Name:** View-Change Flood Amplification
**Product / Version:** FISCO BCOS 3.1.0
**Vendor:** FISCO-BCOS Consortium
**Vulnerability Type:** Other/Unknown (Resource Exhaustion/Logic Flaw)
**Attack Type:** Remote
**Impact:** Denial of Service
**Affected Component(s):** ViewChange Timer Logic

## 2. Description
A flaw in the view-change scheduling mechanism allows a malicious node or an external attacker to trigger excessive view-change events. By exploiting the timer misconfiguration or lack of rate-limiting in `ViewChange` messages, an attacker can flood the network, causing nodes to continuously rotate leaders without finalizing blocks.

## 3. Attack Vectors
- **Flood Malformed ViewChange Messages:** An attacker sends a high volume of `ViewChange` messages that appear valid enough to trigger the timer reset or view rotation logic but prevent actual consensus progress.
- **Exploiting Timer Misconfiguration:** If the view-change timeout is not strictly enforced or increases linearly/exponentially without an upper bound cap being effectively checked against message frequency, the network enters a livelock state.

## 4. Suggested CVE Description
A vulnerability exists in FISCO BCOS 3.1.0 where the view-change scheduling logic does not adequately rate-limit or validate the necessity of view-change requests. This allows a remote attacker to flood the network with malformed or excessive view-change messages, leading to resource exhaustion, continuous leader rotation, and a complete disruption of consensus execution (Denial of Service).

## 5. Validation Information
- **Evidence Required:** Capture a consensus log showing repeated leader rotation (View ID increasing) with no block height progress (`BlockNumber` stagnant).
- **Reproduction:** Use a fuzzer to inject a burst of `ViewChange` messages with increasing View IDs and observe the node's state transition logs.
