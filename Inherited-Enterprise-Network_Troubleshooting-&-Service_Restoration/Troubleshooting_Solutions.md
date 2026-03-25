Troubleshooting Solutions

## Overview

This document summarizes the corrective actions taken to restore full functionality in an inherited enterprise hub-and-spoke network.  
Issues affected routing propagation, DHCP relay behavior, and DNS traffic due to ACL policy restrictions.

Resolution required targeted configuration corrections across routing, service relay, and security policy layers.

---

# Solution 1 — Restore EIGRP Route Advertisement (Branch-B)

**Problem**

Branch-B VLAN 20 subnet was not advertised into the EIGRP routing domain, preventing HQ from learning the return route.

Subnet affected

```
10.14.81.0/24
```

**Fix**

Add the missing subnet to the EIGRP routing process.

```
router eigrp 100
 network 10.14.81.0 0.0.0.255
```

**Verification**

```
show ip route
show ip eigrp neighbors
show ip eigrp topology
ping 10.10.20.100
tracert 10.10.20.100
```

**Result**

• HQ router learned the Branch-B VLAN 20 subnet  
• WAN routing convergence restored  
• Branch-B clients regained service connectivity

---

# Solution 2 — Restore DHCP Relay (Branch-C)

**Problem**

Branch-C router subinterface for VLAN 20 lacked a DHCP relay configuration, preventing client broadcasts from reaching the centralized DHCP server.

**Fix**

Add DHCP relay configuration to the router subinterface.

```
interface g0/0.20
 ip helper-address 10.10.20.100
```

**Verification**

```
show ip interface brief
show running-config
ipconfig /release
ipconfig /renew
ipconfig /all
ping 10.10.20.100
tracert 10.10.20.100
```

**Result**

• DHCP broadcasts forwarded to HQ server  
• Branch-C clients successfully obtained IP leases  
• Default gateway and DNS settings assigned correctly

---

# Solution 3 — Restore DNS Traffic (HQ WAN Router)

**Problem**

ACL policy on the HQ WAN router blocked DNS traffic to the DNS server.

Problematic ACL entries

```
deny udp any host 10.10.20.100 eq domain
deny tcp any host 10.10.20.100 eq domain
```

**Fix**

Remove the DNS deny rules from the ACL to allow queries to reach the server.

**Verification**

```
show access-lists
ping 10.10.20.100
ping srv1.company.local
tracert 10.10.20.100
ipconfig /all
```

**Result**

• DNS queries successfully reached the server  
• Hostname resolution restored across all branch locations  
• Enterprise services dependent on DNS functioned normally

---

# Final Validation

After applying the configuration corrections, the network was validated using routing diagnostics and end-host testing.

```
show ip route
show ip protocols
show ip eigrp neighbors
show ip eigrp topology
show ip interface brief
show access-lists
ping 10.10.20.100
ping srv1.company.local
tracert 10.10.20.100
ipconfig /renew
```

**Operational Status**

• Routing convergence verified across all routers  
• Branch clients successfully obtained DHCP leases  
• DNS hostname resolution functioning normally  
• Stable connectivity confirmed between branch networks and HQ services