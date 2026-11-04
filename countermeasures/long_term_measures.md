# Long-Term Security Measures and Future Recommendations

## 1. Purpose

This document distinguishes the security measures currently implemented and tested in the Zero-Trust IoT network from planned components awaiting integration and recommendations for future development.

The project uses Cisco Packet Tracer to evaluate how network access restrictions, segmentation, and manually applied security responses can improve the protection of IoT devices and network resources.

## 2. Implemented Security Measures

### 2.1 Access Control Lists (ACLs)

The network uses router-based ACLs to enforce access restrictions between IoT devices and protected resources.

The `IOT-POLICY` ACL permits specified communication between authorized devices and destinations while denying traffic that does not match its permit rules.

Testing demonstrated that the Motion Sensor could establish a TCP connection with the IoT Server on port 80 but could not establish the tested connection with the Video Server.

The Rogue Device's unauthorized ICMP communication with the IoT Server was also blocked after ACL enforcement.

### 2.2 Network Segmentation and Quarantine

The network separates IoT devices, security components, servers, and quarantined devices using VLANs.

VLAN 99 serves as the quarantine network.

During testing, the Rogue Device was manually reassigned to VLAN 99. Subsequent ICMP attempts to reach the IoT Server were unsuccessful, and the `QUARANTINE-POLICY` ACL recorded deny-rule matches.

This demonstrates containment of the tested routed communication, although the experiment did not evaluate every possible network destination or protocol.

### 2.3 Manual Countermeasure Enforcement

Restriction, blocking, and isolation were evaluated as separate security responses.

Restriction was demonstrated through destination-specific ACL permissions. Blocking was demonstrated through denied unauthorized ICMP communication. Isolation was demonstrated through manual VLAN reassignment and quarantine ACL enforcement.

These controls were not automatically triggered by calculated trust scores during the completed experiments.

### 2.4 Manual Access Restoration

A controlled restoration procedure was tested by moving the Rogue Device from VLAN 99 back to VLAN 10 and restoring its original IP configuration.

After restoration, the device successfully reached its VLAN 10 gateway (4/4 replies), while communication with the IoT Server remained unsuccessful (0/4 replies).

The experiment demonstrated restoration of gateway connectivity while maintaining the tested access restrictions. Identity verification, remediation checks, and trust-based approval were simulated rather than independently validated.

## 3. Planned Components Awaiting Integration

### 3.1 Manual Trust Scoring and Classification

The project design includes spreadsheet-based trust scoring and device classification using observations collected during network tests.

The finalized scoring rules, thresholds, and classification results are awaiting integration with the countermeasure response procedures.

### 3.2 Device Identity and Authorization Checks

The project also includes device inventory, identity verification, and destination/service authorization rules.

These components are necessary to determine which responses should apply to individual devices and scenarios. Their finalized implementation and validation remain pending.

## 4. Future Recommendations

### 4.1 Automated Trust-Based Enforcement

A future implementation could connect trust-score evaluations directly to network enforcement mechanisms, allowing policy-based restriction or quarantine without requiring an administrator to manually reconfigure switch ports.

### 4.2 Continuous Monitoring and Alerting

Integrating centralized monitoring and alerting could help administrators detect suspicious behavior, track device activity, and respond more quickly to policy violations.

### 4.3 Automated Restoration Approval

Future versions could require verified identity, completed remediation, policy compliance, and acceptable trust scores before automatically restoring access.

### 4.4 Expanded Security Validation

Additional experiments could evaluate more IoT devices, network protocols, destinations, and attack scenarios to determine whether the controls remain effective under different conditions.

## 5. Summary

The current implementation demonstrates static ACL enforcement, destination-specific restrictions, VLAN-based quarantine, and manually performed access restoration.

The remaining project work includes integrating manual trust scoring and authorization decisions into the complete evaluation-and-response workflow.

Automated enforcement, continuous monitoring, alert generation, and automated restoration approval are identified as potential future improvements rather than features already implemented.
