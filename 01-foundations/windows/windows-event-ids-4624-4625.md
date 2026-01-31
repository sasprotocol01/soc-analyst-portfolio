# Windows Event IDs 4624 & 4625 — SOC Analyst Notes

## Overview
Windows authentication activity is recorded in the **Security Event Log**.
For SOC analysts, Event IDs **4624** and **4625** are critical for detecting
unauthorized access, brute force attacks, and compromised credentials.

These events answer the question:
> Who tried to log in, from where, and was it successful?

---

## Windows Event ID 4624 — Successful Logon

### What It Means
Event ID 4624 is generated when a user successfully authenticates to a system.

This does **not automatically mean malicious activity** — context is required.

---

### Key Fields SOC Analysts Review
- **Account Name** – Who logged in
- **Logon Type** – How they logged in
- **Source Network Address** – Where the login came from
- **Timestamp** – When it occurred

---

### Common Logon Types (Important)
- **2** – Interactive (local login)
- **3** – Network (SMB, remote access)
- **10** – RemoteInteractive (RDP)

SOC relevance:
- Logon Type 10 from external IPs requires scrutiny
- Unexpected logon types may indicate misuse

---

## Windows Event ID 4625 — Failed Logon

### What It Means
Event ID 4625 is generated when a login attempt fails.

Single failures are normal.
**Multiple failures are not.**

---

### SOC Red Flags in 4625
- High number of failures in short time
- Attempts against multiple usernames
- Failures followed by a successful 4624
- Source IP from unusual location

---

## Brute Force Detection (SOC Scenario)

### Typical Pattern
1. Multiple **4625** events
2. Same source IP
3. Targeting one or more users
4. Followed by a **4624**

SOC conclusion:
> Credentials may have been guessed or compromised.

---

## Compromised Account Indicators

Suspicious signs:
- Successful login from a new country
- Login outside normal business hours
- Immediate privileged activity after login
- Login followed by unusual process creation

SOC action:
- Escalate for further investigation
- Correlate with process creation (Event ID 4688)

---

## How SOC Analysts Investigate These Events

Step-by-step approach:
1. Identify repeated 4625 events
2. Note source IP and targeted user
3. Look for a matching 4624
4. Check logon type
5. Correlate with other logs (Sysmon, process creation)

SOC mindset:
> One event is noise. Patterns are signals.

---

## Common False Positives

Not all failed logins are attacks.

Examples:
- User mistyped password
- Cached credentials
- Service account password mismatch

SOC job:
- Validate before escalating

---

## Key Takeaways
- 4624 = successful authentication
- 4625 = failed authentication
- Context matters more than single events
- Failed logons followed by success are high-risk
- Correlation is critical in SOC investigations

Understanding these events allows SOC analysts to
identify unauthorized access attempts quickly and accurately.
