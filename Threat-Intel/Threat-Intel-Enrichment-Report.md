# Threat Intelligence Enrichment Report

**Analyst:** Idris Ibrahim Adetola  
**Date:** 13-12-2025  
**Alert Source:** Phishing Email Investigation  
**Related Case:** [Case Study 1 - Phishing](/Case-Studies/Phishing-Email-Analysis.md)

---

## IOCs Analyzed

### 1. IP Address (Command & Control)

| Field | Detail |
|---|---|
| **Value** | `37.120.233.226` |
| **Verdict** | Malicious |
| **Summary** | Active C2 infrastructure associated with Remote Access Trojan (RAT) activity. |

![VirusTotal detection results for IP address](/Screenshots/Threat-Intel/IP-address-threat-intelligence.png)

![AbuseIPDB results for IP address](/Screenshots/Threat-Intel/IP-Threat-intel-(AbuselPDB).png)

---

### 2. URL (Phishing Link)

| Field | Detail |
|---|---|
| **Value** | `https://files-ld.s3.us-east-2.amazonaws.com/59cbd215-76ea-434d-93ca-4d6aec3bac98free-coffee.zip` |
| **Verdict** | Malicious |
| **Summary** | Phishing link and malware payload delivery vector, used to deliver a compressed malicious file. |

![VirusTotal detection results for URL](/Screenshots/Threat-Intel/URL-Threat-intelligence.png)

---

### 3. File Hash (SHA-256)

| Field | Detail |
|---|---|
| **Value** | `CD903AD2211CF7D166646D75E57FB866000F4A3B870B5EC759929BE2FD81D334` |
| **Verdict** | Malicious |
| **Summary** | Flagged by multiple vendors as a Trojan associated with **AsyncRAT**. |

![VirusTotal detection results for SHA-256 hash](/Screenshots/Threat-Intel/File-Hash-Threat-Intelligence.png)

---

## Investigation Context

The IOCs were obtained during a phishing email investigation after detecting outbound communication from the affected endpoint, indicating possible post-compromise C2 activity.

---

## Tools Used For Enrichment

- VirusTotal
- AbuseIPDB
- AlienVault OTX
- GreyNoise (Basic)

---

## Correlation & Analysis

All three IOCs (IP, URL, hash) were linked to the same phishing investigation. Threat intelligence correlation confirms they were part of a coordinated attack chain involving phishing-based delivery followed by post-compromise C2 communication.

---

## Final Risk Assessment

| Factor | Detail |
|---|---|
| **Reputation** | Malicious |
| **Threat Type** | C2 / Remote Access Trojan (RAT) |
| **Likelihood** | High |
| **Impact** | High |
| **Overall Risk Level** | **High** — Confirmed malicious phishing campaign with endpoint compromise |
| **Confidence Level** | High — Multiple threat intelligence sources confirmed malicious classifications |

---

## Recommended Actions

- Block IP address at Firewall and EDR.
- Isolate affected endpoint.
- Perform endpoint malware cleanup.
- User awareness and phishing retraining.
- Escalate incident to Tier 2 SOC for further investigation.