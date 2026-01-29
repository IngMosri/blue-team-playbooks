# Incident Report: Unauthorized Okta Authentication Attempts (IR-2026-004)

## Executive Summary
Investigation into a targeted credential access attempt against a corporate account on January 23, 2026. The attack was characterized by an initial brute-force phase followed by a successful password match. Automated security controls (MFA and Account Lockout) successfully mitigated the threat. No unauthorized access to corporate resources was achieved.

---

## Incident Timeline (UTC)
* **14:30:** Monitoring systems flagged a high volume of failed login attempts for a single user identity.
* **14:45:** Attacker provided a valid credential set. System issued an MFA Push Challenge. The request was ignored by the legitimate user.
* **15:05:** Resumption of brute-force activity from a different source IP, indicating a deliberate attempt to circumvent rate-limiting.
* **15:07:** Account reached maximum failure threshold. Automated Soft Lockout triggered.
* **16:00:** Incident resolution: User identity verified via secondary channel and password reset completed.

---

## Technical Investigation
Analysis of the authentication logs indicates a Credential Stuffing attack. The attacker possessed the correct password, likely sourced from a third-party data breach.

### Indicators of Compromise (IOCs)
| Indicator | Type | Context |
| :--- | :--- | :--- |
| 203.0.113.45 | IPv4 | Primary Attack Source (Commercial ISP) |
| 198.51.100.27 | IPv4 | Secondary Attack Source (VPN/Proxy) |
| user@example-corp.com | Account | Target Identity |

### Forensic Log Extract
| Timestamp | Event Type | Source IP | Status |
| :--- | :--- | :--- | :--- |
| 15:06:21 | login.fail | 203.0.113.45 | INVALID_PASSWORD |
| 15:06:45 | mfa.challenge | 203.0.113.45 | MFA_CHALLENGE_ISSUED |
| 15:07:02 | account.lock | 198.51.100.27 | POLICY_LOCKOUT |

---

## Response and Remediation
1. **Session Termination:** All active sessions for the affected user were cleared globally via the Okta Admin Console.
2. **Out-of-Band Verification:** Contacted the user through corporate Slack. User confirmed the MFA push was unauthorized.
3. **Credential Remediation:** Mandatory password rotation was enforced due to the confirmed compromise of the previous password.
4. **Scope Analysis:** Cross-referenced source IPs against AWS CloudTrail and GitHub audit logs. No additional hits or lateral movement were identified.

---

## Lessons Learned and Recommendations
* **Efficacy:** The existing MFA and Lockout policies performed as designed, preventing an Account Takeover (ATO).
* **Gap:** The presence of a valid password suggests the need for enhanced dark web monitoring for corporate credentials.
* **Strategy:** Recommend transitioning to Phishing-Resistant MFA (WebAuthn/FIDO2) and implementing Conditional Access policies based on geographic anomalies.

### MITRE ATT&CK Mapping
* **T1110.003:** Brute Force (Password Spraying)
* **T1621:** Multi-Factor Authentication Request Generation
* **T1078.004:** Valid Accounts (Cloud Accounts)

---
*Sanitized for public portfolio use.*
