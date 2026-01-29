# Incident Report: Web Enumeration and Reconnaissance Detection (TryHackMe Lab)

## Platform
- Platform: TryHackMe
- Scenario Type: SOC Simulation – Web Application Reconnaissance
- Focus: Blue Team Detection and Incident Response

---

## Executive Summary
This report analyzes a Web Discovery Attack detected during a SOC simulation. An external threat actor attempted to identify hidden administrative directories on a financial web platform. The investigation identified a malicious source IP and resulted in automated mitigation actions to prevent further reconnaissance.

---

## Incident Timeline (UTC)

| Time | Event |
|------|--------|
| 10:21:39 | Attack detected originating from source IP 32.122.195.63 |
| 10:35–10:39 | Multiple 404 and 403 responses observed targeting administrative endpoints |
| 10:45 | Threat Intelligence confirmed the source IP as malicious |
| 10:50 | Mitigation actions applied (IP block and WAF updates) |

---

## Technical Analysis

### Attack Technique: Directory Enumeration
The attacker used automated tools to discover hidden directories by analyzing HTTP response codes. High-frequency 404 and 403 errors indicated directory brute-force attempts.

#### Evidence: URL Discovery Attempts
- https://fakebank.com/admin (404)
- https://fakebank.com/administrator (403)
- https://fakebank.com/login (404)

This behavior is consistent with automated fuzzing tools such as Dirb, Gobuster, or FFUF.

---

## Threat Intelligence

- Source IP: 32.122.195.63 (sanitized)
- Geolocation: Moscow, Russia
- ASN: AS12345 (simulated)
- Reputation: Flagged as malicious with multiple prior reports

---

## Response and Mitigation

### Actions Taken
- **IP Blocking:** Immediate temporary block applied to terminate reconnaissance activity.
- **Rate Limiting:** Request thresholds implemented for administrative endpoints.
- **WAF Update:** New Web Application Firewall rules deployed to detect and block directory enumeration patterns.

---

## Findings
The activity was classified as **Active Web Reconnaissance**. No successful exploitation was observed. Early detection prevented exposure of sensitive administrative endpoints.

---

## Lessons Learned
- Monitoring spikes in 404/403 errors is critical for early reconnaissance detection.
- Threat intelligence enrichment increases confidence in blocking decisions.
- Automated mitigation significantly reduces attack surface during the reconnaissance phase.

---

## MITRE ATT&CK Mapping
- T1595.002: Active Scanning – Vulnerability Scanning  
- T1071.001: Application Layer Protocol – Web Protocols  

---

## Recommendations
- Implement automated directory enumeration detection signatures.
- Enforce strict rate limiting on administrative endpoints.
- Deploy deception directories (honeypots) to detect reconnaissance activity.
- Integrate SIEM correlation rules for HTTP status code anomalies.

---

## Evidence Handling Note
All IP addresses, domains, and artifacts have been sanitized for educational purposes and to comply with confidentiality requirements.
