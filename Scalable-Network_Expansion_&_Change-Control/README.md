# Scalable Network Expansion and Change Control

## Overview

This lab simulates controlled enterprise network expansion within a routed multi-site topology while maintaining routing stability and enforcing scoped access control. The implementation includes HQ migration to Router-on-a-Stick (ROAS), integration of a new branch growth segment, EIGRP route advertisement updates, and enforcement of targeted server access restrictions.

---

## Topology Summary

- HQ VLAN 10 (Users): 10.10.10.0/24
- HQ VLAN 40 (Servers): 10.10.40.0/24
- Branch LAN: 10.10.20.0/24
- Branch Growth LAN: 10.10.30.0/24
- Transit WAN Links: 10.0.12.0/30, 10.0.23.0/30
- Dynamic Routing Protocol: EIGRP AS 100

The design maintains Layer 3 segmentation between sites while enabling dynamic route propagation across all routers.

---

## Architecture Changes

### HQ Migration to Router-on-a-Stick (ROAS)

- Configured 802.1Q trunk between SW1-HQ and R1-HQ
- Implemented router subinterfaces for VLAN 10 and VLAN 40
- Assigned dedicated default gateway for Server VLAN
- Relocated SRV-1 to 10.10.40.100

This migration preserved inter-VLAN routing functionality while improving segmentation and scalability.

---

### Branch Growth Deployment

- Introduced new LAN segment: 10.10.30.0/24
- Advertised network via EIGRP AS 100
- Verified route propagation across R1-R2-R3
- Confirmed routing table consistency and convergence stability

---

### Access Control Policy Enforcement

Configured extended ACL on R3 to restrict branch-to-server communication:

- Deny: 10.10.20.0/24 to 10.10.40.100
- Permit: All other traffic

- Applied ACL inbound on R3 G0/0 (source-facing interface)
- Identified and corrected order-of-operations issue (permit above deny)
- Revalidated policy enforcement through hit counter analysis

---

## Validation and Operational Testing

- Verified EIGRP route propagation for VLAN 40 and Branch Growth networks across R1-R2-R3
- Confirmed permitted end-to-end connectivity remained intact post-change
- Validated targeted denial (10.10.20.0/24 to 10.10.40.100) using controlled traffic testing and ACL hit counters
- Ensured routing stability and absence of unintended traffic disruption

---

## Key Technical Concepts Demonstrated

- VLAN segmentation and gateway reassignment
- Inter-VLAN routing via Router-on-a-Stick (802.1Q trunking and subinterfaces)
- Dynamic routing updates and convergence validation using EIGRP
- Extended ACL design, sequencing, and troubleshooting
- Policy enforcement verification using hit counters
- Structured change control with impact validation

---

## Skills Demonstrated

- VLANs
- Inter-VLAN Routing
- 802.1Q Trunking
- EIGRP
- Extended ACLs
- Routing Table Verification
- Network Segmentation
- Change Control

---

## Outcome Summary


The network expansion was successfully implemented without disrupting existing site connectivity. HQ migration to ROAS improved segmentation and scalability, while integration of the Branch Growth LAN was completed with stable EIGRP route propagation across all routers. Targeted access control was enforced through a correctly sequenced extended ACL, validated using controlled testing and hit counter analysis. Post-change verification confirmed routing stability, preserved permitted traffic flows, and demonstrated effective change control within a multi-site enterprise topology.


