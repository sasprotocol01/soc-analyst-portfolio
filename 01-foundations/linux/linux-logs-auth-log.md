# Linux Logs — /var/log and auth.log (SOC Analyst Notes)

## Overview
Linux logs are a primary source of evidence during investigations.
For SOC analysts, logs answer critical questions about **authentication, system activity, and potential compromise**.

If an action occurred on a Linux system, it was likely recorded in `/var/log`.

---

## The /var/log Directory

`/var/log` is the central location where Linux systems store log files.

SOC analysts do not need to know every log file, but must know **where to look first**.

Common log files include:
- `auth.log`
- `syslog`
- `kern.log`
- `boot.log`

---

## auth.log (CRITICAL FOR SOC)

### What auth.log Records
`auth.log` contains authentication-related events, including:
- Successful logins
- Failed login attempts
- SSH access
- Privilege escalation (sudo)

This log is **high-value** during intrusion investigations.

---

## Key Events Found in auth.log

### Successful Logins
Examples:
- SSH login success
- Local user authentication

SOC questions:
- Who logged in?
- From which IP address?
- Is this user expected on this system?
- Is the login time normal?

---

### Failed Login Attempts
Indicators:
- Repeated authentication failures
- Attempts for non-existent users
- Authentication from unusual IPs

SOC relevance:
- May indicate brute force attacks
- Often precedes successful compromise

---

### SSH Activity
SSH is a common attack vector.

SOC red flags:
- SSH access from unknown IP addresses
- Login attempts outside business hours
- Multiple failures followed by success

---

### Privilege Escalation (sudo)
`auth.log` records when users run commands with elevated privileges.

SOC questions:
- Why did this user need sudo?
- Was the command expected?
- Did this occur shortly after login?

---

## How SOC Analysts Investigate auth.log

Typical investigation steps:
1. Identify suspicious timestamps
2. Review source IP addresses
3. Correlate login success after failures
4. Check user legitimacy
5. Determine potential impact

SOC mindset:
> Authentication activity often tells the story of initial access.

---

## Common SOC Scenarios Using auth.log

### Brute Force Attempt
Indicators:
- Many failed login attempts
- Same source IP
- Targeting the same user or multiple users

---

### Compromised Credentials
Indicators:
- Successful login after repeated failures
- Login from unusual geographic location
- Immediate sudo usage after login

---

## Commands Used by SOC Analysts

Basic commands to review logs:
- `cat auth.log`
- `less auth.log`
- `grep Failed auth.log`
- `grep Accepted auth.log`

SOC focus:
- Filtering for relevant events
- Identifying patterns, not reading everything

---

## Key Takeaways
- `/var/log` is the starting point for Linux investigations
- `auth.log` is critical for authentication analysis
- Failed logins followed by success are suspicious
- Context (user, IP, time) matters more than single events

Understanding Linux logs allows SOC analysts to detect
unauthorized access and investigate attacker behavior effectively.
