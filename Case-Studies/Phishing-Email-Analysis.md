# Case Study 1 — Incident Detection & Alert Triage (Phishing Email)

**Analyst:** Idris Ibrahim Adetola  
**Date:** 12-12-2025

---

## Executive Summary

Phishing alert investigated, confirmed true positive, host contained, and IOCs documented.

---

## Alert Description

A phishing email alert was triggered for an email containing a suspicious attachment and external link, posing a potential threat of malware infection.

---

## 1. Investigation Process

Review of event details included collecting key email metadata:

- Event time
- SMTP address
- Source email address
- Destination email address
- Email subject
- Email delivery status (delivered to recipient)

Using the gathered information, the email security module was accessed and the specific email was located. Upon examination, the email appeared suspicious and contained an attachment and an embedded link.

---

## 2. Evidence Analysis & Enrichment

- The embedded link was extracted and analyzed using VirusTotal, where it was flagged as malicious.
- Email delivery logs confirmed the message was successfully delivered to the recipient.
- Network and log data were examined to determine whether the malicious link was accessed or the attachment executed.

---

## 3. Log & Host Analysis

- The recipient host's IP address was retrieved from the endpoint security module.
- Log management systems were queried using the host IP within the alert timeframe.
- Outbound network connections from the host were identified and analyzed.
- Log correlation confirmed that the user interacted with the malicious link.
- Further analysis revealed communication between the attachment and an external IP address.

---

## 4. Indicators of Compromise (IOCs)

- Malicious external IP address
- Command-and-Control (C2) communication identified
- Threat intelligence enrichment confirmed the IP as associated with **AsyncRAT**

---

## 5. Severity Assessment

| Factor | Detail |
|---|---|
| **Impact** | Potential system compromise and remote access |
| **Likelihood** | High — confirmed user interaction and C2 communication |
| **Severity** | **HIGH** |

---

## 6. Final Decision

> **True Positive:** Confirmed phishing attack with host compromise.

---

## 7. Response and Containment

- The malicious email was removed from the recipient mailbox.
- Endpoint containment actions were applied to prevent further communication.
- Host processes, browser history, terminal activity, and network actions were reviewed to assess the scope of compromise.

---

## See Also

- [Threat Intelligence Enrichment Report](/SOC-Analyst-Portfolio/Threat-Intel/Threat-Intel-Enrichment-Report.md) — IOC enrichment for this case (IP, URL, hash)