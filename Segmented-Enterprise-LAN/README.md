# Segmented Enterprise LAN

## Overview
Designed and implemented a segmented enterprise LAN using VLAN isolation, 802.1Q trunking, router-on-a-stick inter-VLAN routing, extended ACL enforcement, and Layer 2 loop prevention.  
This project emphasizes clean architecture, controlled traffic flow, and verification-driven configuration.

---

## Architecture Summary

### VLANs & Subnets
- VLAN 10 – HR → 192.168.10.0/24
- VLAN 20 – Finance → 192.168.20.0/24
- VLAN 30 – IT → 192.168.30.0/24

### Topology
- 1× Router (router-on-a-stick)
- 2× Layer 2 switches
- Access ports assigned per department
- 802.1Q trunk links between switches and router

Pre- and post-configuration diagrams are located in the `topology/` directory.

---

## Technologies
- Cisco Packet Tracer
- VLANs
- 802.1Q Trunking
- Inter-VLAN Routing (Router-on-a-Stick)
- Extended ACLs
- Spanning Tree Protocol (STP)
- Cisco IOS CLI Verification

---

## Implementation Highlights
- Created and propagated VLANs across multiple switches
- Configured trunk links to carry all required VLANs
- Implemented router subinterfaces for inter-VLAN routing
- Applied extended ACLs to restrict HR access to Finance while permitting authorized traffic
- Verified Layer 2 stability using STP
- Validated routing, policy enforcement, and connectivity through CLI and traffic testing

---

## Verification & Evidence
Ordered screenshots in the `screenshots/` directory document:
- VLAN creation and access port assignment
- Trunk configuration and VLAN propagation
- Router subinterface status
- ACL configuration and match counters
- Inter-VLAN permit/deny behavior
- Spanning Tree verification

Screenshots are numbered to reflect logical review order.

---

## Project Artifacts
- `topology/` — Pre/Post network diagrams
- `screenshots/` — Configuration and verification evidence
- `packet-tracer/` — Packet Tracer source file (`.pkt`)
- `documentation/` — One-page technical summary

---

## Notes
This project is part of an independent network infrastructure portfolio focused on real-world design, verification, and documentation practices. Future projects expand into WAN routing and dynamic routing protocols.