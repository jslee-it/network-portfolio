Inherited Enterprise Network Troubleshooting & Service Restoration

## Overview

This lab simulates troubleshooting an **inherited enterprise network** with multiple operational faults.

The environment includes:

- Hub-and-spoke WAN architecture
- HQ Layer 3 core switching
- Branch router-on-a-stick VLAN segmentation
- Centralized DHCP and DNS services
- EIGRP dynamic routing
- Centralized ACL security policy

Three faults were intentionally injected to simulate realistic network failures.

---

# Network Architecture

### Core Infrastructure

- **HQ-CORE** — Cisco 3560 Layer 3 Switch  
- **HQ-WAN** — Cisco 2911 WAN Edge Router  
- **Branch Routers** — Branch-A, Branch-B, Branch-C  
- **Access Switches** — VLAN segmentation and trunking

### Centralized Services

- **SRV1** — DNS / infrastructure services  
- **SRV2** — internal services  
- **DHCP server** — centralized address management  

Domain: `company.local`

### Routing and Security

- **EIGRP AS 100**
- **WAN ACL: BRANCH-POLICY**

---

# Injected Network Faults

1. Missing **EIGRP route advertisement**
2. Missing **DHCP relay configuration**
3. **DNS traffic blocked by ACL**

---

# Issue 1 — EIGRP Route Advertisement Failure

### Symptom

Branch B VLAN 20 could not reach HQ services due to missing return routing.

### Investigation

Commands used:

```
show ip route
show ip protocols
show ip eigrp neighbors
show ip eigrp topology
show ip interface brief
show running-config
```

### Root Cause

Branch B failed to advertise subnet:

```
10.14.81.0/24
```

Missing configuration:

```
network 10.14.81.0 0.0.0.255
```

### Resolution

Added network statement to EIGRP:

```
router eigrp 100
 network 10.14.81.0 0.0.0.255
```

### Verification

```
show ip route
show ip eigrp neighbors
show ip eigrp topology
ping 10.10.20.100
tracert 10.10.20.100
```

Result: Branch B connectivity restored.

---

# Issue 2 — DHCP Relay Failure

### Symptom

Branch C VLAN 20 clients received APIPA addresses:

```
169.254.x.x
```

### Investigation

Commands used:

```
ipconfig
ipconfig /all
show ip interface brief
show interfaces
show running-config
show ip route
ping 10.10.20.100
tracert 10.10.20.100
show vlan brief
show interfaces trunk
```

### Root Cause

Missing DHCP relay configuration.

Required command:

```
ip helper-address 10.10.20.100
```

### Resolution

Configured helper address:

```
interface g0/0.20
 ip helper-address 10.10.20.100
```

### Verification

```
ipconfig /release
ipconfig /renew
ipconfig /all
ping 10.10.20.100
tracert 10.10.20.100
```

Result: Clients received valid DHCP leases.

Example:

```
IP Address: 172.31.19.x
Default Gateway: 172.31.19.193
DNS Server: 10.10.20.100
```

---

# Issue 3 — DNS Resolution Failure

### Symptom

Clients could reach the DNS server but hostname resolution failed.

```
ping 10.10.20.100
```

Success

```
ping srv1.company.local
```

Failure

### Investigation

Commands used:

```
ping 10.10.20.100
tracert 10.10.20.100
show access-lists
show ip interface brief
show interfaces
show running-config
```

### Root Cause

ACL blocked DNS traffic.

Problematic entries:

```
deny udp any host 10.10.20.100 eq domain
deny tcp any host 10.10.20.100 eq domain
```

### Resolution

Removed DNS deny statements and restored ACL policy.

Example corrected ACL structure:

```
ip access-list extended BRANCH-POLICY
 permit ip any any
```

### Verification

```
show access-lists
ping 10.10.20.100
ping srv1.company.local
tracert 10.10.20.100
ipconfig /all
```

Result: DNS resolution restored.

---

# Key CLI Commands Used

### Routing

```
show ip route
show ip protocols
show ip eigrp neighbors
show ip eigrp topology
```

### Interfaces

```
show ip interface brief
show interfaces
```

### Switching

```
show vlan brief
show interfaces trunk
```

### ACL Verification

```
show access-lists
```

### Configuration Review

```
show running-config
```

### End-Host Testing

```
ping
tracert
ipconfig
ipconfig /all
ipconfig /release
ipconfig /renew
```

---

# Skills Demonstrated

- Network troubleshooting methodology
- EIGRP route advertisement diagnostics
- DHCP relay troubleshooting
- DNS resolution analysis
- ACL policy troubleshooting
- Layer 2 / Layer 3 fault isolation
- Enterprise multi-site network operations

---

# Technologies Used

- Cisco Packet Tracer
- Cisco IOS CLI
- EIGRP
- VLANs
- 802.1Q trunking
- Router-on-a-Stick
- DHCP
- DNS
- Access Control Lists

---

# Project Outcome

All injected faults were successfully identified and resolved through structured CLI troubleshooting.

The network was restored to full operational state with:

- Stable EIGRP routing
- Functional DHCP across all branches
- Reliable DNS resolution
- Correct ACL policy enforcement