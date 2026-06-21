# Case Study 2 - Windows Brute-Force Login Attempt

**Analyst:** Idris Ibrahim Adetola  
**Date:** 19-12-2025  
**Alert Source:** Windows Event Security Log (CSV)  
**SIEM:** Splunk  
**Lab:** BTLO Windows Events

---

## Executive Summary

A brute-force login attack targeting the administrator account was detected in Windows security logs. Multiple audit failure events occurred over a short time window. Using Splunk, missing fields were created using Field Extraction (Regex) to normalize the log for analysis. SPL queries were used to determine the targeted account, source IP, port range, number of failures, and the timeframe of the events. Threat intelligence confirmed the source IP as external, confirming a high-severity attack.

---

## 3. Log Preparation / Field Extraction

- The CSV logs were missing important fields: Source IP, Source Port, Account Name.
- Field extraction (Regex) was used to extract the missing fields.
- This ensured SPL queries returned accurate results for the investigation.

---

## 4. Investigation Findings

### Count of Audit Failures

```splunk
index=main sourcetype="csv"
"Event ID"=4625
TargetUserName="administrator"
| stats count as "Total Audit Failure"
```

**Finding:** `3103` failed login attempts

![Audit Failure count in Splunk](/Screenshots/Case-study/Bruteforce/Audit-failure.png)

---

### Administrator Account Filter

```splunk
index=main sourcetype="csv"
"Event ID"=4625
TargetUserName="administrator"
```

![Administrator filter applied](/Screenshots/Case-study/Bruteforce/administrator-filter.png)

---

### Target Username Field

![TargetUserName field extraction](/Screenshots/Case-study/Bruteforce/TargetUserName.png)

---

### Failure Reasons

```splunk
index=main sourcetype="csv"
"Event ID"=4625
TargetUsername="administrator"
| stats count by Failure_Reason
```

**Finding:** Unknown username or bad password

![Failure reason results](/Screenshots/Case-study/Bruteforce/Failure_reason.png)

---

### Source IP Address

```splunk
index=main sourcetype="csv"
"Event ID"=4625
TargetUsername="administrator"
| stats count by SourceIP
```

**Finding:** Attack originated from `113.161.192.227`

![Source IP results](/Screenshots/Case-study/Bruteforce/Source-IP.png)

---

### Port Range

```splunk
index=main sourcetype="csv"
"Event ID"=4625
TargetUsername="administrator"
| stats min(SourcePort) max(SourcePort)
```

**Finding:** `49162 – 65534`

![Port range results](/Screenshots/Case-study/Bruteforce/Port-Range.png)

---

### Timeline of Failed Attempts

```splunk
index=main sourcetype="csv"
"Event ID"=4625
TargetUsername="administrator"
| timechart count span=5m
```

**Finding:** Attack occurred in bursts over the observed timeframe.

![Login attempt timeline](/Screenshots/Case-study/Bruteforce/Login-attempt-timeline.png)

---

### IP Geolocation

![IP location — Vietnam](/Screenshots/Case-study/Bruteforce/IP-location.png)

---

### Additional Context

| Field | Value |
|---|---|
| **Source IP Geolocation** | Vietnam |
| **Event ID** | 4625 (Failed logon) |
| **Attack Type** | Credential-based Brute-Force attack |

---

## 5. Threat and Impact Assessment

| Factor | Detail |
|---|---|
| **Severity** | High |
| **Impact** | No successful login observed, but credential exposure risk exists |
| **Likelihood** | High — repeated attempts over a short time |

> **Note:** Automated attacker with multiple source ports and external IP.

---

## 6. Recommended Actions

- Block source IP at firewall and endpoint security.
- Implement lockout policy after a few failed attempts on the Administrator account.
- Monitor the targeted account for suspicious activity.
- Escalate incident to Tier 2 SOC for containment or monitoring.

---

## 7. Tools and Methods

| Tool | Usage |
|---|---|
| **Splunk Enterprise** | Log ingestion, Field extraction, SPL queries |
| **Windows Event Log (CSV)** | Source of events |
| **Threat Intelligence** | IP geolocation and reputation lookup |