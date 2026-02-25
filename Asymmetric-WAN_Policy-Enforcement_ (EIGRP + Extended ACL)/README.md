#Asymmetric WAN Policy Enforcement (EIGRP + Extended ACL)

## Overview

This lab demonstrates the deployment of dynamic routing across a multi-router WAN topology using EIGRP, followed by the implementation of asymmetric traffic control using an extended ACL. The objective was to restrict Branch subnet access to a specific HQ server while maintaining permitted inter-site communication and routing stability.

---

## Topology Design

**WAN Architecture**
- Linear routed topology: R1-HQ ↔ R2-Transit ↔ R3-Branch
- Serial point-to-point links (/30 addressing)
- DCE clocking configured on transit router interfaces

**LAN Segments**
- HQ LAN: 10.10.10.0/24
- Branch LAN: 10.10.20.0/24

**Routing Protocol**
- EIGRP AS 100
- Explicit network statements
- `no auto-summary`
- Passive interfaces configured on LAN-facing ports

---

## Implementation Details

### 1. WAN Configuration
- Installed HWIC-2T modules on all routers
- Configured serial interfaces with /30 addressing
- Verified DCE clock rate configuration
- Confirmed interface status using `show ip interface brief`

Result: All WAN interfaces operational (up/up).

---

### 2. EIGRP Deployment
- Enabled EIGRP AS 100 on all routers
- Advertised WAN and LAN subnets
- Configured passive interfaces on LAN-facing ports
- Verified adjacency using: show ip interface brief


Result: All WAN interfaces operational (up/up).

---

### 2. EIGRP Deployment
- Enabled EIGRP AS 100 on all routers
- Advertised WAN and LAN subnets
- Configured passive interfaces on LAN-facing ports
- Verified adjacency using: show ip eigrp neighbors
- Confirmed route propagation using: sho ip route


Result: Successful multi-hop route learning and end-to-end connectivity.

---

### 3. Extended ACL Policy Enforcement

#### Policy Objective
- Deny Branch subnet (10.10.20.0/24) access to HQ-SRV1 (10.10.10.100)
- Permit all other traffic

#### Configuration Approach
- Created named extended ACL on R3
- Applied outbound on Serial0/0/0 (toward HQ)
- Enforced source-proximal policy placement

#### Validation
- Branch → HQ-SRV1: Denied
- Branch → HQ-PC: Allowed
- HQ → Branch: Allowed
- Confirmed hit counters using: show access-lists


Result: Asymmetric WAN traffic control successfully enforced without disrupting permitted communication.

---

### 4. EIGRP Metric Manipulation

To demonstrate understanding of composite metric calculation:

- Captured baseline feasible distance for 10.10.20.0/24
- Modified delay on R2 Serial0/0/1
- Observed metric increase in routing table
- Validated recalculated feasible distance

Example:
- Baseline metric: 2,682,112
- Post-delay metric: 7,290,112

Result: Verified delay influence on EIGRP route cost and composite metric recalculation.

---

## Verification Commands Used:
-show ip interface brief
-show ip eigrp neighbors
-show ip route
-show ip route 10.10.20.0
-show access-lists


---

## Skills Demonstrated

- Multi-router EIGRP configuration and validation
- WAN serial deployment with DCE clock rate management
- Extended ACL placement strategy and enforcement
- Asymmetric inter-site traffic control
- EIGRP composite metric manipulation
- CLI-based troubleshooting and validation

---

## Outcome

Successfully integrated dynamic routing and access control within a multi-hop WAN environment. Demonstrated controlled traffic enforcement while maintaining routing adjacency and inter-site connectivity. This lab reinforces practical enterprise networking concepts including routing protocol deployment, policy-based traffic control, and metric engineering.

