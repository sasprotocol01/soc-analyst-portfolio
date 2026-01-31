# Windows Event ID 4688 — Process Creation (SOC Analyst Notes)

## Overview
Windows Event ID **4688** is generated when a **new process is created**.
For SOC analysts, this event is one of the most valuable sources of evidence
for detecting malware execution, attacker activity, and post-compromise behavior.

If malware runs on a Windows system, **4688 tells the story**.

---

## What Event ID 4688 Records

Event ID 4688 logs:
- The **newly created process**
- The **parent process**
- The **user account** that executed it
- The **command line** (if enabled)
- The **timestamp**

SOC analysts rely on this event to understand **how execution occurred**.

---

## Why Process Creation Matters in SOC

Attackers must execute code to:
- Run malware
- Launch scripts
- Establish persistence
- Move laterally

SOC principle:
> Execution is where intent becomes visible.

---

## Key Fields SOC Analysts Review

### New Process Name
- What executable ran?
- Is it a known Windows binary or something unusual?

Examples:
- `powershell.exe`
- `cmd.exe`
- `mshta.exe`
- Unknown executables in temp directories

---

### Parent Process (CRITICAL)
The parent process shows **what launched the process**.

Normal examples:
- `explorer.exe` → user applications
- `services.exe` → system services

Suspicious examples:
- `winword.exe` → `powershell.exe`
- `excel.exe` → `cmd.exe`
- Browser → script interpreter

SOC insight:
> Parent-child relationships reveal attacker techniques.

---

### Command Line
The command line shows **how the process was executed**.

SOC red flags:
- Encoded PowerShell commands
- Download-and-execute patterns
- Obfuscated parameters
- Execution from temp directories

---

## Common Malicious Execution Patterns

### Office Spawning PowerShell
Pattern:



SOC interpretation:
- Likely malicious document
- Possible phishing attack
- Requires immediate investigation

---

### Script Execution from Temporary Locations
Examples:
- `C:\\Users\\*\\AppData\\Local\\Temp`
- `C:\\Windows\\Temp`

SOC concern:
- Malware often executes from writable directories

---

### Living-off-the-Land Binaries (LOLBins)
Attackers abuse legitimate tools like:
- `powershell.exe`
- `cmd.exe`
- `mshta.exe`
- `rundll32.exe`

SOC challenge:
- Tools are legitimate
- **Usage is malicious**

---

## How SOC Analysts Investigate Event ID 4688

Step-by-step approach:
1. Identify suspicious process
2. Review parent process
3. Analyze command line
4. Check execution location
5. Correlate with user activity
6. Look for follow-up actions

SOC mindset:
> One process is a clue. The chain is the evidence.

---

## Correlation with Other Events

Event ID 4688 becomes more powerful when combined with:
- **4624 / 4625** — Who logged in
- **Sysmon Event ID 1** — Enhanced process details
- Network logs — Outbound connections

SOC rule:
> No single log tells the full story.

---

## Common False Positives

Not all suspicious-looking processes are malicious.

Examples:
- IT scripts
- Administrative tools
- Software updates

SOC responsibility:
- Validate before escalating
- Use context and corroborating logs

---

## Key Takeaways
- Event ID 4688 tracks process execution
- Parent-child relationships are critical
- Command line analysis reveals intent
- Execution chains expose attacker behavior
- Correlation separates noise from incidents

Mastering Event ID 4688 allows SOC analysts to
identify malicious activity early and accurately.
