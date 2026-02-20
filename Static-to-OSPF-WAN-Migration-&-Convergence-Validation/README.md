# Static to OSPF WAN Migration & Convergence Validation

## Overview
Designed and implemented a three-router WAN topology migrating from static routing to OSPF (Area 0).  
This project emphasizes controlled routing migration, redundant path engineering, equal-cost multi-path validation, and deterministic convergence under simulated link failure.

---

## Architecture Summary

### WAN & LAN Subnets
- 10.1.12.0/30 – R1 ↔ R2  
- 10.1.23.0/30 – R2 ↔ R3  
- 10.1.13.0/30 – R1 ↔ R3  
- 172.16.10.0/24 – Site A  
- 172.16.20.0/24 – Site B  
- 172.16.30.0/24 – Site C  

### Topology
- 3× Routers (triangular WAN core)
- /30 point-to-point WAN links
- Single OSPF Area 0
- Manual Router ID assignment
- Passive LAN interfaces to limit unnecessary adjacency formation

Post-migration topology diagram is located in the `topology/` directory.

---

## Technologies
- Cisco Packet Tracer
- Static Routing
- OSPF (Single Area – Area 0)
- Equal-Cost Multi-Path (ECMP)
- Manual Router ID Configuration
- Passive Interfaces
- Cisco IOS CLI Verification

---

## Implementation Highlights
- Established full static routing baseline with verified inter-site connectivity
- Enabled OSPF Process 1 and advertised WAN/LAN networks
- Manually assigned Router IDs for deterministic control-plane behavior
- Configured passive interfaces on LAN segments
- Removed static routes following successful adjacency formation
- Validated ECMP behavior under normal operation
- Simulated WAN link failure to observe SPF recalculation and path collapse
- Confirmed adjacency restoration and routing stability post-recovery

---

## Verification & Evidence
Ordered screenshots in the `screenshots/` directory document:
- OSPF neighbor formation (FULL state)
- ECMP route installation (dual next-hops)
- Controlled failover and path recalculation
- Static route removal validation
- Final OSPF routing table integrity

Screenshots are numbered to reflect logical review order.

---

## Project Artifacts
- `topology/` — Final OSPF topology diagram
- `screenshots/` — Configuration and verification evidence
- `packet-tracer/` — Packet Tracer source file (`.pkt`)
- `documentation/` — One-page technical summary

---

## Notes
This project builds on foundational LAN segmentation by expanding into WAN routing migration and dynamic protocol deployment.  
Subsequent labs introduce advanced routing protocols and traffic control across multi-router topologies.
