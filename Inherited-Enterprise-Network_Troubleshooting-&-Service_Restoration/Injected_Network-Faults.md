# Lab 5 — Inherited Enterprise Network (Injected Faults)

## Overview

This lab simulates troubleshooting an **inherited enterprise network** with multiple simultaneous infrastructure faults across routing, policy, DHCP relay, and DNS services.

The objective is to restore connectivity while preserving the intended security policy between user and server networks.

---

# Injected Faults

| Fault | Device | Layer | Impact |
|------|------|------|------|
| DNS blocked by ACL | R_HQ-WAN | Policy | Hostname resolution fails |
| Missing EIGRP advertisement | R_Branch-B | Routing | Branch-B VLAN 20 unreachable |
| DHCP scope missing DNS option | SRV1 | Service | Clients cannot resolve hostnames |
| Missing DHCP relay | R_Branch-C | DHCP | Branch-C VLAN 20 clients fall to APIPA |

---

# Fault 1 — ACL Blocking DNS

## Device
`R_HQ-WAN`

### Fault
```
ip access-list extended BRANCH-POLICY
10 deny udp any host 10.10.20.100 eq domain
20 deny tcp any host 10.10.20.100 eq domain
```

### Fix
```
conf t
ip access-list extended BRANCH-POLICY
no 10
no 20
end
```

### Result
DNS queries to `SRV1` allowed again.

---

# Fault 2 — Missing EIGRP Advertisement

## Device
`R_Branch-B`

### Fault

Branch-B VLAN 20 network not advertised to EIGRP.

Missing:

```
10.14.81.0/24
```

### Fix
```
conf t
router eigrp 100
network 10.14.81.0 0.0.0.255
end
```

### Result
Return routing restored for Branch-B VLAN 20.

---

# Fault 3 — DHCP Scope Missing DNS Option

## Device
`SRV1`

### Fault

Clients received IP addresses but no DNS server:

```
DNS Servers : 0.0.0.0
```

### Fix

Set DHCP DNS option:

```
DNS Server: 10.10.20.100
```

### Result
Clients can resolve `srv1.company.local`.

---

# Fault 4 — Missing DHCP Relay

## Device
`R_Branch-C`

### Fault

VLAN 20 subinterface missing helper address.

```
interface g0/0.20
encapsulation dot1Q 20
ip address ...
```

### Fix

```
conf t
interface g0/0.20
ip helper-address 10.10.20.100
end
```

### Result
Branch-C VLAN 20 clients receive DHCP leases.

---

# Validation

Branch user tests after repairs:

```
ping 10.10.20.100
```

Result

```
Success
```

```
ping srv1.company.local
```

Result

```
Success
```

```
ping 10.10.20.101
```

Result

```
Fail (ACL policy enforced)
```

---

# Skills Demonstrated

- Multi-layer network troubleshooting
- EIGRP routing diagnostics
- ACL analysis and correction
- DHCP relay troubleshooting
- DHCP scope validation
- DNS service validation
- Router-on-a-stick verification

---

# Verification Commands

Packet Tracer compatible troubleshooting commands used:

```
show ip route
show ip protocols
show ip eigrp neighbors
show ip interface brief
show vlan brief
show interfaces trunk
show access-lists
ipconfig
ping
tracert
```

---

# Author

**Independent Networking & Infrastructure Projects**  
Enterprise Network Troubleshooting Lab Series