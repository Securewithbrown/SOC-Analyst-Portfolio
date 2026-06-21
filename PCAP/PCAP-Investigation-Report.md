# PCAP Investigation Report - Network Traffic Analysis

**Analyst:** Idris Ibrahim Adetola  
**Date:** 06-02-2026  
**Case ID:** PCAP-101  
**Severity:** High

---

## 1. Objective

Analyze captured network traffic within a PCAP file to identify:

- Indicators of Compromise (IOCs)
- Anomalous communication patterns
- Evidence of malware payload delivery
- Post-infection activities or Command-and-Control behaviours

---

## 2. Tools and Data Sources

| Tool | Purpose |
|---|---|
| **Wireshark** | Network traffic analyzer |
| **PCAP Source** | Malware-Traffic-Analysis.net (Public Dataset) |
| **Threat Intelligence** | urlscan.io, virustotal.com |

---

## 3. PCAP Overview

Observed traffic included DNS requests and responses, TCP sessions between internal hosts and external IPs, and HTTP/HTTPS request and response traffic.

---

## 4. Investigation Methodology

### 4.1 Protocol Identification

Primary protocols analyzed: DNS, TCP, HTTP/HTTPS.

### 4.2 DNS Analysis

Multiple suspicious domains were identified that did not align with known legitimate services. These domains were enriched using open-source threat intelligence and flagged as malicious.

### 4.3 IP Address Analysis

- Repeated, structured outbound connections to the same external IPs.
- Multiple TCP communications consistent with C2 behaviour.
- Threat intelligence confirmed several IPs as associated with malicious infrastructure.

### 4.4 HTTP and HTTPS Review

- Multiple POST and GET requests to malicious domains — potential payload delivery or C2 instructions.
- A GET request traced to the victim's PowerShell terminal contained obfuscated PowerShell code — potential payload delivery.
- Multiple structured POST requests at ~30-second intervals after the GET request — consistent with **C2 beaconing**.
- POST packet sizes varied between **350 and 450 bytes** — the malware transmits small data chunks per cycle, not static beacons.

---

## 5. Evidence Collected

### Malicious Domains

1. `dng-microsoftds.com`
2. `windows-msgas.com`
3. `event-time-microsoft.org`
4. `varying-rentals-calgary-predict.trycloudflare.com`

### Malicious IP Addresses

| IP Address |
|---|
| 172.67.146.241 |
| 104.21.96.1 |
| 104.21.80.1 |
| 104.21.64.1 |
| 104.21.24.186 |
| 104.21.16.1 |
| 104.21.112.1 |
| 104.16.231.132 |
| 104.16.230.132 |

### Wireshark Filters Used

```
dns
ip.addr == {suspicious IP}
http.request || tls.handshake.type eq 1
```

*Follow TCP Stream* was also used to inspect traffic content.

---

## 6. Screenshots

### First Malicious Traffic Observed

![First malicious traffic in Wireshark](/Screenshots/PCAP-Analysis/First-malicious-traffic.png)

### Malicious Domains Identified

![Malicious domains identified in Wireshark](/Screenshots/PCAP-Analysis/Malicious-Domains.png)

### Structured POST Requests to Malicious Domains

![Structured POST requests with consistent interval - C2 beaconing](/Screenshots/PCAP-Analysis/Structured-POST-request-to-malicious-domains.png)

---

## 7. Key Findings

| Finding | Status |
|---|---|
| Traffic types observed | DNS, TCP, HTTP, HTTPS |
| Malicious domains identified | ✓ |
| Malicious IP communication detected | ✓ |
| Possible malware payload download observed | ✓ |
| Evidence of potential C2 activities | ✓ |

---

## 8. Conclusion and Recommended Actions

The PCAP analysis reveals multiple indicators of compromise including malicious DNS resolution and suspicious outbound communications consistent with C2 behaviour. The findings strongly suggest malware infection and post-compromise activities on the affected host.

**Recommended Actions:**

1. Block identified malicious IP addresses and domains at the Firewall and network security controls.
2. Isolate affected endpoint from the network.
3. Perform deep forensics on the affected host.
4. Perform endpoint malware remediation and full system scanning.
5. Review user activities and provide security awareness training if applicable.
6. Escalate incident to Tier 2 SOC for deeper forensic analysis.