# Case Study 3 - SOC Case Study: Malware Network Compromise (BTLO PCAP Analysis)

**Analyst:** Idris Ibrahim Adetola  
**Date:** 21/01/2026  
**Platforms / Tools:** Wireshark, VirusTotal, AlienVault OTX  
**Data Source:** BTLO "Network Analysis – Malware Compromise" PCAP File  
**Case Type:** Malware Download → C2 Communication

---

## 1. Alert Summary

Unusual outbound network traffic was detected from an internal host, suggesting malware infection and potential C2 communication.

---

## 2. Host & Network Identification

| Field | Value |
|---|---|
| **Victim IP** | 10.11.27.101 |
| **Suspicious Domains** | klychenogg.com, cochrimato.com |
| **External IPs (Payload Download)** | 95.181.198.231, 176.32.33.108 |
| **External C2 IP** | 185.244.150.230 |

---

## 3. PCAP / Network Investigation

- DNS filter used to identify abnormal domain queries.
- HTTP GET filter used to identify malware payload downloads.
- TCP stream analysis revealed repeated C2 beaconing behaviour.

### DNS Traffic Analysis

![DNS traffic filter in Wireshark](/Screenshots/Case-study/PCAP-Analysis/DNS-traffics.png)

### Domain Reputation Check

![Domain reputation check - VirusTotal / AlienVault](/Screenshots/Case-study/PCAP-Analysis/Domains-reputation-check.png)

### HTTP GET Filter - Malware Download

![HTTP GET filter identifying malware payload downloads](/Screenshots/Case-study/PCAP-Analysis/HTTP-GET-filter-to-identify-malware-download.png)

### Outbound Traffic Filter (Above 100 Bytes)

![Filter of outbound traffic above 100 bytes](/Screenshots/Case-study/PCAP-Analysis/Filter-of-outbound-traffic-above-100-Byte.png)

### Malware File Hash

![Malware file hash - VirusTotal results](/Screenshots/Case-study/PCAP-Analysis/Malware_File_Hash.png)

---

## 4. Timeline of Events

| Time | Event |
|---|---|
| 16:30:14 | First DNS request to suspicious domain |
| 16:30:15 | HTTP GET request for malware payload |
| 16:30:34 | Second DNS request to suspicious domain |
| 16:30:37–16:30:41 | HTTP GET requests for multiple malware payloads |
| 16:38:57 | First outbound C2 connection |
| 16:38:58 | Repeated beaconing observed |

---

## 5. Threat Intelligence

- IP and domain reputation confirmed malicious.
- Malware file hash flagged as Trojan and spyware.
- IOC correlation confirmed post-compromise attack chain.

---

## 6. Impact & Risk Assessment

| Factor | Detail |
|---|---|
| **Severity** | High |
| **Threat Type** | Trojan and spyware |
| **Likelihood** | High |
| **Impact** | High |
| **Risk Level** | **Critical** |

---

## 7. Response & Containment Actions

- Isolated affected endpoint from the network.
- Blocked external malicious C2 IP at the firewall and EDR.
- Escalated incident to Tier 2 SOC.

---

## 8. Skills Demonstrated

- PCAP Traffic Analysis
- Threat Intelligence Correlation
- Network Forensics
- IOC Extraction and Validation
- Incident Documentation and Escalation