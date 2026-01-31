# Phase 1 — Windows Fundamentals for SOC

> **Goal:** Understand Windows internals from a SOC analyst perspective.
> **Outcome:** Be able to analyze Windows logs, processes, and startup behavior to detect suspicious activity.

---

## 1. Windows Internals (SOC View)

Windows investigations focus on **processes, services, and persistence mechanisms**.

SOC Core Idea:

> Every action on Windows creates evidence — your job is to find and interpret it.

---

## 2. Processes & Parent Processes

### What Is a Process?

* A running instance of a program
* Each process has:

  * Process ID (PID)
  * Parent Process ID (PPID)
  * User context
  * Execution path

---

### Parent–Child Relationships (CRITICAL)

Processes do not appear randomly.

**Normal Examples**

* `explorer.exe` → `chrome.exe`
* `services.exe` → `svchost.exe`

⚠️ **SOC Red Flags**

* `winword.exe` → `powershell.exe`
* `excel.exe` → `cmd.exe`
* `powershell.exe` running Base64 commands

SOC Question:

> Does this parent process logically make sense?

---

## 3. Windows Services

### What Are Services?

* Background processes
* Often start automatically at boot

Examples:

* Windows Update
* Event Log
* Defender

---

### SOC Concerns with Services

⚠️ Red Flags:

* Newly created services
* Services running from non-standard paths
* Services with random names

SOC Tip:

> Attackers love services because they provide persistence.

---

## 4. Windows Registry (Startup Locations)

### What Is the Registry?

* Central configuration database
* Stores startup and system behavior

---

### Common Startup Registry Keys

* `HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Run`
* `HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Run`

**SOC Usage**

* Identify persistence mechanisms
* Detect malware auto-start entries

⚠️ Red Flags:

* Executables in Temp or AppData directories
* Obfuscated or random registry value names

---

## 5. Important Windows Logs

### Security Log (MOST IMPORTANT)

Contains:

* Authentication events
* Process creation
* Privilege changes

Used for:

* Account compromise detection
* Malware execution tracking

---

### System Log

Contains:

* OS-level events
* Service start/stop
* Driver issues

SOC Usage:

* Detect system instability
* Identify suspicious service behavior

---

### Application Log

Contains:

* Application-specific errors
* Software crashes

SOC Usage:

* Identify exploitation attempts
* Application abuse

---

## 6. Key Windows Event IDs (CRITICAL)

### Event ID 4624 — Successful Logon

Tracks:

* Who logged in
* From where
* Logon type

SOC Red Flags:

* Logons at unusual times
* Logons from unexpected IPs
* High-privilege accounts logging in remotely

---

### Event ID 4625 — Failed Logon

Tracks:

* Failed authentication attempts

SOC Red Flags:

* Repeated failures (brute-force)
* Multiple users targeted from same source

SOC Insight:

> Many attacks succeed right after many failures.

---

### Event ID 4688 — Process Creation

Tracks:

* Newly created processes
* Parent process
* Command line

SOC Red Flags:

* Office spawning PowerShell
* Execution from `Temp` or `AppData`
* Encoded or obfuscated command lines

---

## 7. SOC Investigation Questions

Every Windows alert should answer:

* What process started this?
* What is the parent process?
* Who executed it?
* Where is the binary located?
* Is this behavior normal for this system?
