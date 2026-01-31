# Ports and Services — SOC Analyst Notes

## Overview
In a SOC environment, ports and services help analysts understand **what type of activity is occurring** on a network.

Every network connection uses a port, and each port usually maps to a specific service.
Understanding what is *normal* is critical for identifying what is *suspicious*.

---

## What Is a Port?
A port is a logical number that identifies a specific service or application running on a system.

Ports allow multiple services to communicate over the same IP address.

---

## Common Ports Every SOC Analyst Must Know

| Port | Service | Normal Usage |
|-----|--------|-------------|
| 22  | SSH    | Remote administration (Linux/Unix) |
| 80  | HTTP   | Unencrypted web traffic |
| 443 | HTTPS  | Encrypted web traffic |
| 3389 | RDP   | Remote Desktop (Windows) |
| 53  | DNS    | Domain name resolution |

Memorizing these ports is non-negotiable for SOC work.

---

## SOC Perspective: Normal vs Suspicious

SOC analysts do not ask:
> “Is this port open?”

They ask:
> “Does this port make sense **for this system**?”

### Examples

- A **web server** using port 443 → Normal  
- A **user workstation** listening on port 3389 → Suspicious  
- A **domain controller** making outbound DNS requests → Normal  
- A **user workstation** making DNS requests to random external IPs → Suspicious  

---

## High-Risk Ports (SOC Attention Required)

### RDP (Port 3389)
- Frequently targeted by attackers
- External exposure is a major risk
- Multiple failed login attempts may indicate brute force

SOC Action:
- Check source IP
- Check authentication logs
- Verify if RDP exposure is expected

---

### DNS (Port 53)
DNS is heavily abused by attackers for:
- Command & Control (C2)
- Data exfiltration
- Domain Generation Algorithms (DGA)

SOC Red Flags:
- High volume of DNS requests
- Random-looking domain names
- DNS traffic going to non-DNS servers

---

## Why Outbound Traffic Matters More Than Inbound

Inbound traffic:
- Often blocked by firewalls
- Easier to detect

Outbound traffic:
- Indicates successful execution
- Often shows command & control communication

SOC Rule:
> If a system is talking out when it shouldn’t, investigate.

---

## Key Takeaways
- Ports identify **what service is in use**
- Context matters more than port number
- DNS and RDP require extra scrutiny
- Outbound traffic often signals compromise

Understanding ports and services allows SOC analysts to quickly assess
whether activity is expected or potentially malicious.
