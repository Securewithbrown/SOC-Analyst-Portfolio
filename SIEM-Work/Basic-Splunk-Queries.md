# Splunk Search Language (SPL) - Basic Detection Queries

**Analyst:** Idris Ibrahim Adetola  
**Date:** 17-12-2025  

---

## Overview
This report documents the development and validation of three Splunk Processing Language (SPL) queries designed to identify suspicious file creation, process execution, and network communication activity within Sysmon logs. The goal is to demonstrate basic detection engineering skills, noise reduction techniques, and investigative workflows used in a SOC environment.

I performed basic log analysis using Splunk Processing Language to identify suspicious process creation, network connections, and suspicious file creation.

---

### • Query 1: Suspicious file creation

```splunk
index=main sourcetype=_json EventID=11
| eval Target=lower(TargetFilename)
| where (like(Target, "%\temp\%") AND (like(Target,"%.exe") OR like(Target,"%.dll")))
| where NOT like(Target,"%\program files\%")
| where NOT like(Target,"%\windows\%")
| table _time, user, computer, TargetFilename, Image, ProcessID
| sort _time
```

![Screenshot: Splunk search interface displaying results for Query 1, showing columns for _time, user, computer, TargetFilename, Image, and ProcessID indicating .dll and .exe creations in temp directories.](/Screenshots/SIEM-Work/suspicious-file-creation.png)

#### Explanation:
| Line | Purpose |
|---|---|
| Line 1 | Filters for EventID = 11 (Sysmon File Create). |
| Line 2 | Creates `Target` field in lowercase to avoid case-sensitive misses. |
| Line 3 | Filters files in `Temp` directory ending with `.exe` or `.dll`. |
| Lines 4–5 | Reduces noise by excluding benign paths (Program Files, Windows). |
| Line 6 | Outputs results in table form with relevant fields. |
| Line 7 | Sorts output by time of events. |

---

### • Query 2: Suspicious Process Creation

```splunk
index=main sourcetype=_json EventID=1 Image IN ("*powershell.exe", "*cmd.exe", "*wscript.exe", "*cscript.exe")
| eval PI = lower(ParentImage), PC=lower(ParentCommandLine)
| eval score=0
| eval score=score + if(like(PI,"%\Downloads\%") OR like(PI,"%\temp\%"),1,0)
| eval score=score + if(like(PC,"%powershell%") OR like(PC,"%cmd.exe%") OR like(PC,"%wscript%") OR like(PC,"%cscript%"),1,0)
| where score >= 1
| table _time, host, user, Image, ParentImage, ProcessId, ParentProcessId, ParentCommandLine, score
| sort _time
```

![Screenshot: Splunk search panel showing Query 2 execution and two event rows where powershell.exe spawned cmd.exe with risk scores of 1.](/Screenshots/SIEM-Work/Suspicious-process-creation.png)

#### Explanation:

| Line | Purpose |
|---|---|
| Line 1 | Filters EventID = 1 (process creation) for `powershell`, `cmd`, `wscript`, `cscript`. |
| Line 2 | Creates lowercase `PI` and `PC` fields for case-insensitive matching. |
| Line 3 | Initializes `score = 0` as a simple risk-scoring mechanism. |
| Line 4 | Adds 1 point if parent process came from Downloads or Temp. |
| Line 5 | Adds 1 point if parent command-line contains scripting keywords. |
| Line 6 | Keeps only events with score ≥ 1 (at least one suspicious behaviour). |
| Line 7 | Displays results in table format. |
| Line 8 | Sorts results by time. |


---

### • Query 3: Network Connections By IP

```splunk
index=main sourcetype=_json EventID=3
| stats count by destinationIp
| sort - count
```

![Screenshot: Splunk Statistics tab showing an aggregated table of destinationIp addresses sorted in descending order by event count.](/Screenshots/SIEM-Work/Network-connection-filter.png)

#### Explanation:
This query aggregates network connection events by destination IP and sorts them by frequency to identify potential Command-and-Control (C2) infrastructure, beaconing behavior, or abnormal outbound traffic patterns.
