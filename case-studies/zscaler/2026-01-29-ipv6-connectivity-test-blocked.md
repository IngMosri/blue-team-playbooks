# Incident Report: Zscaler IPv6 Connectivity Test Correlation (IR-2026-012)

## Executive Summary
Investigation into a series of blocked outbound connections flagged by Zscaler as "Invalid Server IP." The activity was detected during a proactive log review on January 28, 2026. Analysis confirmed that the traffic was not malicious but rather a False Positive caused by Windows Network Connectivity Status Indicator (NCSI) attempting to reach IPv6 endpoints through the corporate proxy.

---

## Incident Timeline (UTC)
* **12:03:** Initial detection of blocked traffic to `ipv6.msftconnecttest.com` originating from a "Road Warrior" client.
* **12:15:** Correlation analysis started to compare blocked logs with concurrent successful M365 authentication events.
* **12:30:** Identification of the destination as a legitimate Microsoft NCSI endpoint used for captive portal detection.
* **12:45:** Verification of Zscaler IPv6 policy; confirmed that link-local addresses (fe80::/64) are blocked by design.
* **13:00:** Case closed as Benign/False Positive. No user impact reported.

---

## Technical Analysis
The investigation focused on determining if the blocked traffic represented a C2 beaconing attempt or an OS-level function. The correlation of logs showed that while the user was successfully authenticating into Outlook and Microsoft 365, the Windows OS was simultaneously attempting to validate IPv6 internet reachability.

### Indicators Analyzed
| Indicator | Type | Context | Status |
| :--- | :--- | :--- | :--- |
| `ipv6.msftconnecttest.com` | Domain | MS Connectivity Test | **Benign** |
| `fe80::/64` (Sanitized) | IPv6 | Link-Local Address | **Internal/Blocked** |

### Correlated Audit Logs
| Timestamp | Destination | Category | Action | Reason |
| :--- | :--- | :--- | :--- | :--- |
| 12:03:11 | `login.microsoftonline.com` | Authentication | **Allowed** | Valid User Login |
| 12:03:13 | `ipv6.msftconnecttest.com` | Infrastructure | **Blocked** | Invalid Server IP |
| 12:27:45 | `ipv6.msftconnecttest.com` | Infrastructure | **Blocked** | Invalid Server IP |

---

## Response and Remediation
1. **Log Correlation:** Cross-referenced Zscaler logs with endpoint telemetry to confirm the requests originated from the Windows `System` process, not a browser or third-party executable.
2. **Policy Verification:** Validated that the Zscaler Client Connector was functioning correctly and that the "Blocked" action was a result of the proxy's inability to route IPv6 link-local traffic to the public internet.
3. **User Impact Check:** Verified with the Service Desk; no "No Internet" icons or VPN connectivity issues were reported by the user, confirming the block did not break the workflow.
4. **False Positive Documentation:** Updated the internal SOC knowledge base to document this specific NCSI behavior to prevent future escalations.

---

## Lessons Learned and Recommendations
* **Efficacy:** The SWG policy is correctly preventing non-routable IPv6 traffic from leaving the network, which is a sound security posture.
* **Analysis Insight:** This case highlights the importance of not analyzing blocked logs in isolation. The presence of concurrent, legitimate M365 traffic was the key to quickly identifying the activity as benign.
* **Recommendation:** Consider adding Microsoft NCSI endpoints to the Zscaler "PAC file" bypass list if these logs create excessive noise in the SIEM.

### MITRE ATT&CK Mapping
* **T1071.001:** Application Layer Protocol: Web Protocols (Standard OS behavior).

---
*Sanitized for public portfolio use.*
