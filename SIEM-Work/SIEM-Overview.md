# SIEM Overview

**Analyst:** Idris Ibrahim Adetola  
**Date:** 16-12-2025

---

## What is a SIEM?

Security Information and Event Management (SIEM) is a security platform that collects, centralizes, analyzes, and correlates logs from multiple security systems across an organization to detect security threats and generate actionable alerts.

---

## Uses and Functions of SIEM

- Data collection and aggregation
- Log normalization
- Event correlation
- Dashboard creation and monitoring
- Alerting and notification

---

## Importance of a SIEM

SIEM is critical in SOC operations because it:

- Reduces incident detection time
- Centralizes logs from multiple systems for a holistic view
- Generates alerts when detection rules are triggered or suspicious activity is detected
- Helps analysts quickly identify specific threats
- Speeds up incident investigation
- Correlates activity across systems

---

## How Logs Flow in a SOC Environment

### 1. Log Generation

Logs are generated from multiple sources, including:

- Windows and Linux systems (authentication events, process events)
- Network devices (firewalls, proxies, routers)
- Security tools (IPS/IDS, email security, web security, EDR)
- Cloud services and applications

### 2. Log Collection and Ingestion

Logs are forwarded to the SIEM in real or near-real time using log agents, Syslog, or APIs and cloud connectors.

### 3. Parsing and Normalization

The SIEM parses logs to ensure data from different sources is consistent and structured for analysis.

---

## Correlation and Detection

The SIEM applies detection rules, correlation logic, and behavioral patterns. Example detections:

- Multiple failed login attempts followed by a successful login
- Outbound connection to a known malicious IP address
- Suspicious PowerShell execution

---

## Alert Details

Alerts represent potential security incidents (not confirmed threats). Each alert contains:

- Source and destination details
- Timestamps
- Matched detection rule
- Severity level

---

## Tier-1 SOC Analyst Daily Tasks with SIEM

- Validate alerts (True / False positives)
- Review triggered detection rules
- Investigate logs and related events
- Enrich alerts using threat intelligence
- Escalate confirmed incidents to Tier-2 SOC
- Monitor security alerts
- Close false positives with proper documentation

---

## SIEM Flow Summary

| Stage | Description |
|---|---|
| **Log Sources** | Windows/Linux events, network devices, security tools, cloud services |
| **SIEM** | Centralizes, parses, normalizes, and stores logs |
| **Correlation & Detection** | Applies detection rules, behavioral patterns, and correlation logic |
| **Alerts Generated** | Potential security incidents flagged by severity |
| **SOC Analyst** | Reviews, investigates, enriches, escalates, or closes alerts |

---

## Splunk Screenshots

![Splunk dashboard](../Screenshots/SIEM/Splunk-Dashboard.png)

![Splunk search query](../Screenshots/SIEM/Splunk-Search.png)

![Splunk search results 2](../Screenshots/SIEM/Splunk-Search2.png)

---

## Conclusion

SIEM is the core of SOC operations, collecting and correlating logs from across an organization and generating actionable alerts that analysts use to detect and respond to threats efficiently.