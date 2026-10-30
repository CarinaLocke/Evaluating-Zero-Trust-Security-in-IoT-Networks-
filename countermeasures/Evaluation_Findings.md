# Countermeasure Evaluation Findings

## 1. Unauthorized Access Before and After ACL Enforcement

During baseline testing, the Rogue Device (`192.168.10.50`) attempted to communicate with the IoT Server (`192.168.30.10`) using ICMP. Before ACL enforcement, the device received three successful replies out of four attempts, demonstrating that the tested communication was possible without the security policy.

After the `IOT-POLICY` ACL was applied, the same communication attempt resulted in zero successful replies out of four. A subsequent retest also resulted in zero successful replies.

These observations demonstrate that ACL enforcement prevented the tested unauthorized ICMP communication while maintaining a consistent blocking result across the two post-enforcement runs.

Additional testing demonstrated that the Motion Sensor (`192.168.10.11`) could establish an authorized TCP connection to the IoT Server on port 80. However, a complete HTTP application-layer response was not verified.

The Rogue Device was also assigned to VLAN 99, where its attempted ICMP communication with the IoT Server resulted in four unsuccessful replies. The router's `QUARANTINE-POLICY` ACL recorded four matches on its deny rule, supporting the conclusion that the quarantine policy blocked the tested routed traffic.

These results provide initial evidence that the configured security controls can distinguish between permitted and prohibited network communication. However, the findings are limited to the devices, protocols, and scenarios tested.
