# Proposed Countermeasure Mapping

**Status:** Draft — Proposed for Team Review  
**Project:** Evaluating Zero-Trust Security in IoT Networks

## 1. Overview

This document outlines a preliminary mapping of security scenarios to proposed countermeasure responses for our Zero-Trust IoT network.

The proposed responses are based on the security controls and network behaviors evaluated so far. They are intended to provide a starting point for defining how the network should respond to authorized activity, unauthorized access attempts, abnormal behavior, and repeated security policy violations.

**Note:** These mappings are proposals only and have not yet been finalized, approved, or fully implemented. Additional team input is required before establishing the final countermeasure policies.

## 2. Proposed Security Response Mapping

| Scenario | Proposed Response | Reason |
|---|---|---|
| Registered device communicating with an authorized server | **Allow** | Legitimate traffic should remain accessible. |
| Registered device communicating with an unauthorized destination | **Block + Alert** | Registration alone does not authorize access to every destination. |
| Device displaying abnormal network behavior | **Restrict + Alert** | Limit access while investigating suspicious activity. |
| Unknown or unregistered device attempting to access protected resources | **Block + Alert** | Device identity has not been verified. |
| Device repeatedly violating security policies | **Isolate + Alert** | Repeated violations may justify stronger containment measures. |
| Device classified as untrusted | **Isolate** | Separate the device from protected network resources. |

## 3. Preliminary Considerations

The proposed responses represent different levels of security enforcement:

- **Allow:** Permit network communication when the device and destination satisfy the required security conditions.
- **Restrict:** Limit device access or communication privileges when abnormal behavior is detected.
- **Block:** Deny specific unauthorized communication attempts.
- **Isolate:** Separate a device from protected network resources when it presents a significant security concern.
- **Alert:** Generate a security notification or log entry to document suspicious activity or policy violations.

These responses may be used individually or in combination, depending on the nature and severity of the security event.

## 4. Pending Team Input

Before finalizing the countermeasure mapping, the following components must be incorporated:

- **Trust-Scoring Thresholds (Bryan):** Determine how device trust scores will influence the selection of security responses.
- **Authorization Definitions (Nephie):** Establish which devices, destinations, and communication activities are considered authorized or unauthorized.
- **Countermeasure Evaluation:** Review whether the proposed responses can be implemented and tested within the existing Cisco Packet Tracer environment.

## 5. Current Status and Next Steps

This document serves as an initial proposal for discussion and further development.

The countermeasure mappings will be reviewed and revised as the team finalizes its trust-scoring thresholds, authorization requirements, and network security policies.

**Final implementation and validation are still pending.** The proposed responses should not be interpreted as fully implemented or experimentally verified security controls.
