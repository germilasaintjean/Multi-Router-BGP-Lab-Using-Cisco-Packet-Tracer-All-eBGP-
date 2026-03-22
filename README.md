### 📘 **Border Gateway Protocol (BGP) Multi‑Router Lab – Cisco Packet Tracer**
Built a multi‑router BGP lab in Cisco Packet Tracer to simulate inter‑domain routing across multiple autonomous systems. Configured BGP neighbors, advertised networks, verified route propagation, and achieved full end‑to‑end connectivity between hosts in different ASes.

This project demonstrates how to build and configure a multi‑router BGP topology in Cisco Packet Tracer. The goal was to simulate inter‑domain routing across four autonomous systems and achieve full end‑to‑end connectivity between two hosts located in different networks. The lab includes IP addressing, BGP neighbor configuration, route advertisement, troubleshooting, and verification.

---

### 🗺️ **Network Topology**

```
PC1 — R1 — R2 — R3 — R4 — PC2
```
<img width="1368" height="256" alt="image" src="https://github.com/user-attachments/assets/861f5c2a-7a26-4e84-8b0e-dea290661950" />


| Device | Interface | IP Address | Description |
|--------|-----------|------------|-------------|
| PC1 | NIC | 10.1.1.10/24 | Host in first LAN |
| R1 | G0/0 | 10.1.1.1 | PC1 LAN |
| R1 | G0/1 | 10.1.12.1 | Link to R2 |
| R2 | G0/0 | 10.1.12.2 | Link to R1 |
| R2 | G0/1 | 10.1.23.2 | Link to R3 |
| R3 | G0/0 | 10.1.23.3 | Link to R2 |
| R3 | G0/1 | 10.2.34.3 | Link to R4 |
| R4 | G0/0 | 10.2.34.4 | Link to R3 |
| R4 | G0/1 | 10.2.2.1 | PC2 LAN |
| PC2 | NIC | 10.2.2.10/24 | Host in second LAN |

---

### 🧩 **Autonomous System Assignments**

Packet Tracer does **not support iBGP**, so each router uses its own AS number:

| Router | AS |
|--------|----|
| R1 | 65001 |
| R2 | 65002 |
| R3 | 65003 |
| R4 | 65004 |

This ensures all BGP sessions operate as eBGP, which Packet Tracer supports.

---

### 🔧 **Full CLI Configuration**

### **R1 – AS 65001**
```
enable
configure terminal

router bgp 65001
 bgp log-neighbor-changes
 neighbor 10.1.12.2 remote-as 65002
 network 10.1.1.0 mask 255.255.255.0

end
```

---

### **R2 – AS 65002**
```
enable
configure terminal

router bgp 65002
 bgp log-neighbor-changes
 neighbor 10.1.12.1 remote-as 65001
 neighbor 10.1.23.3 remote-as 65003

end
```

---

### **R3 – AS 65003**
```
enable
configure terminal

router bgp 65003
 bgp log-neighbor-changes
 neighbor 10.1.23.2 remote-as 65002
 neighbor 10.2.34.4 remote-as 65004

end
```

---

### **R4 – AS 65004**
```
enable
configure terminal

router bgp 65004
 bgp log-neighbor-changes
 neighbor 10.2.34.3 remote-as 65003
 network 10.2.2.0 mask 255.255.255.0

end
```

---

## 🧪 **Verification Commands**

### Check BGP neighbors:
```
show ip bgp summary
```

### Check BGP routes:
```
show ip bgp
show ip route
```

### End‑to‑end connectivity:

From **PC1**:
```
ping 10.2.2.10
```

From **PC2**:
```
ping 10.1.1.10
```

Both succeeded.

###⚠️ **Challenges Faced**

### **1. Packet Tracer does not support iBGP**
Attempting to configure neighbors in the same AS produced:
```
%Cisco Packet Tracer does not support internal BGP in this version.
```
**Solution:**  
Assigned each router a unique AS so all sessions became eBGP.

---

### **2. Duplicate IP Address Error**
```
%IP-4-DUPADDR: Duplicate address 10.2.34.3
```
**Cause:**  
R4 was accidentally configured with the same IP as R3.

**Fix:**  
Corrected R4 G0/0 to 10.2.34.4.

---

### **3. First ping failing**
Initial pings showed:
```
.!!!!
```
**Cause:** ARP resolution delay.  
**Fix:** Subsequent pings succeeded normally.

---

### 🎯 **What I Learned**

- How to configure BGP neighbors across multiple autonomous systems  
- How to advertise networks using the `network` command  
- How to verify BGP neighbor states and routing tables  
- How to troubleshoot BGP issues (Idle, Active, Established)  
- How ARP affects initial connectivity  
- How to adapt network designs when simulators have limitations  
- How multi‑router routing works end‑to‑end  

