# Countermeasure Response Plan

**Project:** Evaluating Zero-Trust Security in IoT Networks  
**Contributor:** Maria  
**Status:** Preliminary definitions — implementation testing in progress

## 1. Restriction

Restriction involves limiting an IoT device's network access to approved destinations and services rather than denying all communication. In our Cisco Packet Tracer network, the `IOT-POLICY` access control list (ACL) is configured on router subinterface `GigabitEthernet0/0.10` to permit specific IoT devices to communicate with their designated servers over HTTP while denying other routed traffic. This supports the Zero-Trust principle of least-privilege access.

## 2. Blocking

Blocking involves denying unauthorized communication attempts to prevent devices from accessing prohibited network resources. Our `IOT-POLICY` ACL contains a `deny ip any any` rule, which denies traffic that does not match an earlier permit rule.

During testing, the Rogue Device (`192.168.10.50`) successfully received 3 out of 4 ping replies from the IoT Server (`192.168.30.10`) before ACL protection. After ACL protection, the same test received 0 out of 4 successful replies. This provides evidence that the configured ACL prevented the tested ICMP communication.

## 3. Isolation

Isolation involves separating suspicious or untrusted devices from protected network resources until they can be investigated and verified. Our network includes a quarantine subnet associated with VLAN 99 (`192.168.99.0/24`).

The `QUARANTINE-POLICY` ACL is configured inbound on router subinterface `GigabitEthernet0/0.99` and contains a `deny ip any any` rule. This is intended to prevent devices assigned to the quarantine VLAN from sending IP traffic through the router.

However, device reassignment to VLAN 99 and verification of isolation have not yet been tested. Communication within the same VLAN may require additional controls.

## 4. Next Steps

- Confirm trust-scoring classifications and thresholds with Bryan.
- Confirm authorized destinations and services with Nephie.
- Test legitimate communication after ACL enforcement.
- Test restriction, blocking, and isolation separately.
- Develop and test an access-restoration procedure.
- Document results and supporting screenshots.
