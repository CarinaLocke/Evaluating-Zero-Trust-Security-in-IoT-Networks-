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
## 3. Countermeasure Repeatability Evaluation

To evaluate the consistency of the implemented security controls, several network communication experiments were repeated in Cisco Packet Tracer using the same configurations.

**Before ACL Enforcement:** The Rogue Device (`192.168.10.50`) successfully communicated with the IoT Server (`192.168.30.10`) in both the original baseline test and the repeat test. The original test received three out of four successful ping replies, while the recorded repeat test received four out of four. An initial attempt during the repeat session received one out of four replies before a subsequent attempt succeeded completely. This variation may have been caused by network initialization or address resolution, although the exact cause was not verified.

**After ACL Enforcement:** The Rogue Device received zero successful ping replies in both the original and repeated tests, demonstrating consistent blocking of the tested unauthorized ICMP communication.

**Authorized Communication:** The Motion Sensor (`192.168.10.11`) successfully established a TCP connection with the IoT Server (`192.168.30.10`) on port 80. The connection was also observed to succeed during a repeat test, although supporting screenshots from the second run were not saved.

**Quarantine Enforcement:** After the Rogue Device was moved into VLAN 99, both the original and repeated ping tests resulted in zero successful replies. The `QUARANTINE-POLICY` ACL recorded four deny-rule matches during the original test and eight cumulative matches after the retest.

### Evaluation Summary

The results demonstrate that the tested access-control and quarantine mechanisms consistently prevented the specified unauthorized ICMP communication while permitting the tested authorized TCP connection.

However, these findings are limited to the specific devices, traffic types, and configurations evaluated. The experiments do not establish that every unauthorized protocol is blocked, that complete HTTP application transactions succeed, or that automated trust-based responses and alerts are functioning.

Additional testing and integration with the team's finalized trust-scoring and authorization policies will be necessary to evaluate the complete Zero-Trust framework.
## 4. Week 4: Countermeasure Implementation and Restoration Testing

### 4.1 Restriction Testing

To evaluate least-privilege access restrictions, the Motion Sensor (`192.168.10.11`) attempted to establish a TCP connection on port 80 with the Video Server (`192.168.30.20`), which was not an authorized destination under the existing `IOT-POLICY` ACL.

During simulation, the packet reached R1-Gateway, where the ACL matched the `deny ip any any` rule. The packet was subsequently denied and dropped.

This result was compared with the previously successful TCP connection between the Motion Sensor and its authorized IoT Server (`192.168.30.10`).

Together, these observations demonstrate that the existing ACL permits the tested authorized communication while restricting access to an unauthorized destination.

**Evidence:** `Restriction_MotionSensor_VideoServer_ACL_Deny.png`

### 4.2 Blocking Testing

Blocking effectiveness was evaluated through previously conducted experiments involving the Rogue Device (`192.168.10.50`) and IoT Server (`192.168.30.10`).

Before ACL enforcement, the Rogue Device successfully communicated with the IoT Server. After ACL enforcement, both the original and repeated ICMP tests resulted in zero successful replies out of four attempts.

These findings demonstrate consistent blocking of the tested unauthorized ICMP communication. No additional ACL configuration changes were required during Week 4.

### 4.3 Isolation Testing

Isolation was evaluated by assigning the Rogue Device's switch port (`SW1 Fa0/4`) to quarantine VLAN 99 and configuring the device with IP address `192.168.99.50` and default gateway `192.168.99.1`.

Attempts to reach the IoT Server resulted in zero successful ICMP replies. The router's `QUARANTINE-POLICY` ACL recorded deny-rule matches, increasing from four to eight cumulative matches following repeat testing.

These findings support the effectiveness of the quarantine configuration in blocking the tested routed communication.

### 4.4 Restoration Procedure Testing

A controlled restoration experiment was conducted to evaluate whether a quarantined device could regain network connectivity without automatically receiving unrestricted access to protected resources.

**Initial State:** The Rogue Device was assigned to VLAN 99 with IP address `192.168.99.50` and gateway `192.168.99.1`. A pre-restoration ping to the IoT Server returned zero successful replies.

**Configuration Changes:**

1. Reassigned switch port `SW1 Fa0/4` from VLAN 99 to VLAN 10.
2. Changed the Rogue Device's IP address from `192.168.99.50` to `192.168.10.50`.
3. Changed its default gateway from `192.168.99.1` to `192.168.10.1`.
4. Preserved the existing ACL configurations without modification.

**Post-Restoration Results:**

| Test | Result |
|---|---|
| Rogue Device → R1-Gateway (`192.168.10.1`) | 4/4 successful ICMP replies |
| Rogue Device → IoT Server (`192.168.30.10`) | 0/4 successful ICMP replies |

The results demonstrate that the Rogue Device regained connectivity to its original VLAN 10 gateway while remaining unable to communicate with the IoT Server through the tested ICMP protocol.

This supports the principle that restoring network connectivity does not automatically grant unrestricted access to protected resources.

**Evidence:**
- `Restoration_Gateway_Ping_Success.png`
- `Restoration_IoT_Server_Access_Denied.png`
- `Restoration_Test.pkt`

### 4.5 Limitations

The countermeasures were implemented and evaluated using static ACL configurations and manual VLAN reassignment in Cisco Packet Tracer.

Although the experiments demonstrated destination-specific restriction, unauthorized ICMP blocking, quarantine enforcement, and restored gateway connectivity, they did not verify automated trust-score calculations, automated alerts, identity verification, or remediation-based restoration approval.

The restoration procedure simulated administrator authorization rather than implementing an automated Zero-Trust decision process. Additional integration and testing would be necessary to validate these mechanisms.

### 5.1 Week 5: Isolation Verification Across Protected Servers

To further evaluate the effectiveness of VLAN-based isolation, the quarantined Rogue Device (`192.168.99.50`) was tested against the Video Server (`192.168.30.20`) using ICMP ping requests.

The device remained assigned to VLAN 99 through switch port Fa0/4, and the `QUARANTINE-POLICY` ACL remained applied inbound on router subinterface `G0/0.99`. No configuration changes were required.

The Video Server test resulted in 0/4 successful replies (100% packet loss). The router's quarantine ACL also displayed four cumulative matches for the `deny ip any any` rule.

Together with the earlier IoT Server isolation test, these findings demonstrate that the quarantined Rogue Device could not successfully communicate with either protected server using the tested ICMP traffic.

The results support the effectiveness of the configured quarantine controls for these routed communication attempts. However, they do not establish isolation effectiveness for every protocol or all possible destinations.
