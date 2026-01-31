# Phase 1 — Linux Fundamentals for SOC

> **Goal:** Understand how Linux systems work from a SOC analyst perspective.
> **Outcome:** Be able to identify suspicious processes, users, permissions, and logs during an investigation.

---

## 1. Processes in Linux (ps, top)

### What a Process Is

* A running instance of a program
* Every process has:

  * PID (Process ID)
  * Parent PID (PPID)
  * User who executed it
  * Command/path

---

### `ps` Command (Process Snapshot)

Common usage:

* `ps aux`

Shows:

* USER → who ran the process
* PID → process ID
* %CPU / %MEM → resource usage
* COMMAND → what is running

**SOC Usage**

* Identify suspicious processes
* Find unexpected services
* Check execution path

⚠️ SOC Red Flags

* Processes running from `/tmp`, `/dev/shm`, `/var/tmp`
* Commands with random names
* Processes running as `root` without justification

---

### `top` Command (Live View)

* Real-time view of running processes
* Useful for spotting:

  * High CPU usage
  * Resource abuse
  * Cryptominers

**SOC Tip**

> Sudden spikes in CPU or memory often indicate malware activity.

---

## 2. Files & Permissions

### Linux File Permissions

Each file has:

* Owner
* Group
* Permission set (rwx)

Example:

```
-rwxr-xr--
```

Meaning:

* Owner → read, write, execute
* Group → read, execute
* Others → read only

---

### SOC Permission Concerns

⚠️ Red Flags:

* World-writable files
* Executables in user home directories
* SUID/SGID binaries modified recently

SOC Question:

> Should this user be able to execute this file?

---

## 3. System Logs (`/var/log`)

### What `/var/log` Contains

* System activity records
* Authentication attempts
* Service logs

Common log files:

* `/var/log/auth.log` → authentication events
* `/var/log/syslog` → general system activity
* `/var/log/messages` → system messages (RHEL-based)

---

### `auth.log` (CRITICAL)

Tracks:

* SSH logins
* `sudo` usage
* Failed authentication attempts

**SOC Usage**

* Detect brute-force attacks
* Identify compromised accounts
* Track privilege escalation

⚠️ SOC Red Flags

* Multiple failed logins
* Logins at unusual times
* Successful login after many failures

---

## 4. Users and Groups

### Users

* Defined in `/etc/passwd`
* System users vs human users

SOC Checks:

* Unexpected new users
* Users with UID 0 (root-level access)

---

### Groups

* Defined in `/etc/group`
* Control permissions and access

SOC Red Flags:

* User added to `sudo` or `wheel` group unexpectedly
* Privilege escalation without change request

---

## 5. SOC Thinking (Investigation Mindset)

Every Linux investigation should answer:

* **What process started this?**
  → Check PID, PPID, command path

* **Who executed it?**
  → Identify user account

* **Where is it running from?**
  → Look for execution from temp or hidden directories

> Processes, users, and logs always tell a story — your job is to read it.

---

## Status

✅ Phase 1 — Linux Fundamentals for SOC: **COMPLETE**
