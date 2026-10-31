# Countermeasure Evaluation Findings

## 1. Unauthorized Access Before and After ACL Enforcement

During baseline testing, the Rogue Device (`192.168.10.50`) attempted to communicate with the IoT Server (`192.168.30.10`) using ICMP. Before ACL enforcement, the device received three successful replies out of four attempts, demonstrating that the tested communication was possible without the security policy.

After the `IOT-POLICY` ACL was applied, the same communication attempt resulted in zero successful replies out of four. A subsequent retest also resulted in zero successful replies.

These observations demonstrate that ACL enforcement prevented the tested unauthorized ICMP communication while maintaining a consistent blocking result across the two post-enforcement runs.

Additional testing demonstrated that the Motion Sensor (`192.168.10.11`) could establish an authorized TCP connection to the IoT Server on port 80. However, a complete HTTP application-layer response was not verified.

The Rogue Device was also assigned to VLAN 99, where its attempted ICMP communication with the IoT Server resulted in four unsuccessful replies. The router's `QUARANTINE-POLICY` ACL recorded four matches on its deny rule, supporting the conclusion that the quarantine policy blocked the tested routed traffic.

These results provide initial evidence that the configured security controls can distinguish between permitted and prohibited network communication. However, the findings are limited to the devices, protocols, and scenarios tested.

## 2. Mapping Suspicious Scenarios to Security Responses

The Zero-Trust security model uses different countermeasures depending on device identity, authorization, network behavior, and policy violations. The proposed responses include allowing legitimate communication, restricting suspicious activity, blocking unauthorized requests, and isolating devices that present a greater security risk.

| Scenario | Proposed Security Response | Explanation |
|---|---|---|
| Registered device accessing an authorized destination | Allow | Permit communication that follows the device's approved access permissions. |
| Registered device accessing an unauthorized destination | Block and Alert | Prevent unauthorized communication even when the device is recognized. |
| Device demonstrating abnormal network behavior | Restrict and Alert | Limit network access while the suspicious behavior is investigated. |
| Unknown or unregistered device attempting to access protected resources | Block and Alert | Deny access until the device's identity and authorization are verified. |
| Device repeatedly violating security policies | Isolate and Alert | Move the device into a quarantine network to prevent further unauthorized routed communication. |
| Device classified as untrusted | Isolate | Separate the device from protected network resources until restoration requirements are satisfied. |

### Relationship to Experimental Results

The experiments conducted in Cisco Packet Tracer provide evidence for several of these responses.

**Allow:** The Motion Sensor successfully established a TCP connection with the IoT Server on port 80, demonstrating that the ACL permitted the tested authorized communication.

**Block:** After the `IOT-POLICY` ACL was applied, the Rogue Device received zero successful ping replies from the IoT Server. The blocking result was also reproduced during a subsequent test.

**Isolate:** The Rogue Device was reassigned to VLAN 99, and its attempted ICMP communication with the IoT Server was unsuccessful. The `QUARANTINE-POLICY` ACL recorded four deny-rule matches.

**Restrict:** The `IOT-POLICY` ACL implements destination- and service-specific permissions. However, a separate experiment evaluating restricted access for a suspicious but authorized device has not yet been completed.

**Alert:** Alert generation and logging have not yet been verified experimentally.

### Limitations and Pending Team Decisions

These response mappings are preliminary and must be reviewed against the team's finalized trust-scoring classifications and authorization policies.

Bryan's scoring criteria will determine how device behavior and trust classifications influence the selected response. Nephie's authorization rules will establish which devices, destinations, and services are permitted.

Until these details are confirmed, the proposed mappings should not be treated as finalized or fully automated security decisions.
