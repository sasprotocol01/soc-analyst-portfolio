# Phase 1 — Networking for SOC

> **Goal:** Build a SOC analyst mindset for analyzing network traffic.
> **Outcome:** Be able to explain *what happened, why it matters, and whether it is suspicious*.

---

## 1. IP Address & MAC Address (SOC Perspective)

### IP Address

* Logical identifier used for communication across networks
* Can change (DHCP, VPN, cloud)

**SOC Usage**

* Identify source and destination of traffic
* Correlate alerts, firewall logs, and SIEM events

**SOC Red Flags**

* Internal IP contacting many external IPs
* Traffic to malicious or unexpected geolocations
* Sudden spike in outbound connections

---

### MAC Address

* Physical hardware identifier
* Used only within local networks

**SOC Usage**

* Identify devices inside LAN
* Detect rogue or spoofed devices

⚠️ If the same IP appears with different MAC addresses → possible spoofing or new device

---

## 2. TCP vs UDP (Security View)

### TCP (Connection-Oriented)

* Reliable communication
* Uses 3-way handshake (SYN, SYN-ACK, ACK)

**Common TCP Services**

* HTTP / HTTPS
* SSH
* FTP

**SOC Notes**

* Easier to track sessions
* Clear start and end of communication

---

### UDP (Connectionless)

* No handshake
* Faster, less reliable

**Common UDP Services**

* DNS
* NTP
* VoIP

**SOC Risks**

* Harder to track
* Frequently abused in:

  * DDoS attacks
  * DNS tunneling
  * Data exfiltration

---

## 3. Ports and Services (Critical Knowledge)

| Port | Service | Normal Use            |
| ---- | ------- | --------------------- |
| 22   | SSH     | Remote administration |
| 53   | DNS     | Name resolution       |
| 80   | HTTP    | Web traffic           |
| 443  | HTTPS   | Encrypted web traffic |
| 3389 | RDP     | Windows remote access |

**SOC Rule**

> Always ask: *Is this service normal for this port?*

⚠️ Example:

* PowerShell traffic over port 443 → suspicious
* Database traffic over port 80 → suspicious

---

## 4. DNS, HTTP, HTTPS (SOC Core Protocols)

### DNS (Port 53)

**Purpose:** Resolve domain names to IP addresses

**Why attackers abuse DNS**

* Almost always allowed through firewalls
* Low inspection
* Can hide data inside queries

**SOC Red Flags**

* Long, random domain names
* High volume of DNS requests
* Repeated failed lookups

---

### HTTP (Port 80)

* Unencrypted traffic

**SOC Concerns**

* Malware downloads
* Cleartext credential exposure
* Command-and-control communication

---

### HTTPS (Port 443)

* Encrypted traffic

**SOC Challenge**

* Payload cannot be inspected easily
* Must rely on:

  * Destination
  * Frequency
  * Timing
  * Certificate anomalies

> Encryption protects users — and attackers.

---

## 5. Internal vs External Traffic

### Internal Traffic

* Occurs within the organization

**SOC Risks**

* Lateral movement
* Internal scanning
* Credential abuse

---

### External Traffic

* Traffic entering or leaving the organization

**SOC Risks**

* Data exfiltration
* Malware command-and-control
* Suspicious outbound connections

> SOC truth: outbound traffic is often more dangerous than inbound.

---

## 6. SOC Analyst Core Questions

For every alert, always ask:

* Why is this traffic suspicious?
* Is this normal for this port?
* Is this normal for this protocol?
* Is this normal for this user or host?

---

## 7. Knowledge Check (Must Answer Confidently)

**What does port 443 traffic normally mean?**

* Encrypted HTTPS web traffic
* Can also hide malware communication or data exfiltration

**Why is DNS used for attacks?**

* Widely trusted
* Rarely blocked
* Low visibility
* Can covertly transfer data

---

## Status

✅ Phase 1 — Networking for SOC: **COMPLETE**

Next: **Linux Fundamentals for SOC**
