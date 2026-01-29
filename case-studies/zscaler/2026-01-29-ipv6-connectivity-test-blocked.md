# Zscaler Blocked Traffic Investigation – IPv6 Connectivity Test Correlation Case Study

## Scenario Overview
During routine proxy log monitoring, multiple outbound connections were observed and blocked due to invalid server IP classification. The activity coincided with legitimate Microsoft 365 authentication and productivity traffic, requiring correlation analysis to determine whether the behavior was malicious or benign.

---

## Detection Source
- Tool: Zscaler Secure Web Gateway (SWG)
- Policy Action: Blocked
- Category: Internet Services / Unknown
- Client Type: Zscaler Client Connector (Road Warrior)

---

## Initial Detection
The event was detected during proactive log review in the Zscaler dashboard. Multiple entries were observed within the same time window, including:

- Allowed Microsoft 365 authentication traffic
- Allowed corporate SaaS productivity traffic
- Blocked IPv6 connectivity test traffic (ipv6.msftconnecttest.com)

The blocked entries were flagged due to **invalid server IP classification**.

---

## Initial Triage
- Destination domain: ipv6.msftconnecttest.com
- Destination IP: fe80::/64 (IPv6 link-local address, sanitized)
- Policy Action: Blocked due to invalid server IP
- Observed concurrently with Microsoft authentication and collaboration traffic
- No user-reported suspicious activity

---

## Investigation Steps

### 1. Log Review
- Reviewed Zscaler proxy logs for the affected endpoint and time window.
- Identified concurrent legitimate Microsoft 365 traffic (login.microsoftonline.com, outlook.office365.com).
- Confirmed blocked traffic originated from Windows system processes.

---

### 2. Destination and OS Behavior Analysis
- Identified ipv6.msftconnecttest.com as Microsoft Network Connectivity Status Indicator (NCSI) endpoint.
- Confirmed Windows uses this domain to verify internet connectivity and captive portal detection.
- Determined the traffic was OS-generated, not user-initiated.

---

### 3. Policy Validation
- Confirmed Zscaler policy blocks IPv6 link-local addresses (fe80::/64) as invalid external destinations.
- Determined the blocking behavior was expected based on IPv6 policy configuration.

---

### 4. Traffic Correlation Analysis
Concurrent Microsoft 365 authentication and productivity traffic was observed, including:

- login.microsoftonline.com
- outlook.office365.com
- Microsoft security telemetry endpoints

Correlation confirmed this traffic was consistent with normal corporate user activity. The blocked ipv6.msftconnecttest.com requests were identified as OS-level connectivity checks and unrelated to authentication workflows.

No indicators of account compromise, suspicious login patterns, or session hijacking were detected.

---

### 5. User Impact Validation
- No user disruption or connectivity issues were reported.
- Normal browsing and SaaS access remained functional.

---

## Simulated Correlated Log Sample

| Timestamp (UTC) | Destination | Category | Action |
|----------------|-------------|----------|--------|
| 2026-01-28 12:03:11 | login.microsoftonline.com | Professional Services | Allowed |
| 2026-01-28 12:03:12 | outlook.office365.com | Webmail | Allowed |
| 2026-01-28 12:03:13 | ipv6.msftconnecttest.com | Internet Services | Blocked |
| 2026-01-28 12:27:45 | ipv6.msftconnecttest.com | Unknown | Blocked |

---

## Findings
The blocked traffic was classified as **benign Windows operating system connectivity test behavior**. The blocking occurred due to proxy policy handling of IPv6 link-local addresses and did not indicate malicious activity or endpoint compromise.

---

## Lessons Learned
- Windows performs automatic connectivity validation using Microsoft-controlled endpoints.
- IPv6 link-local addresses can trigger proxy false positives.
- Log correlation is required to differentiate OS noise from real threats.
- SaaS authentication traffic may appear concurrently and must not be misinterpreted as compromise.

---

## MITRE ATT&CK Mapping
- T1071.001: Web Protocols (benign OS connectivity validation)

---

## Recommendations
- Review IPv6 handling policies in secure web gateway configurations.
- Allow Microsoft connectivity test domains if appropriate to reduce noise.
- Implement correlation rules to reduce false positives during OS connectivity checks.
- Train SOC analysts to distinguish OS-generated traffic from user-initiated browsing.

---

## Evidence Handling Note
All domains, IP addresses, and logs have been sanitized to comply with corporate confidentiality and privacy requirements.
