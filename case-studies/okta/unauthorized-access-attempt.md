# [IR-2026-004] Okta Unauthorized Access Attempt Case Study

## 📝 Scenario Overview
On **January 23, 2026**, between **14:30 and 16:00**, a targeted credential access attempt was detected against a corporate identity account. The security system identified a transition from failed logins to a valid password submission, which triggered an MFA challenge. The attack was successfully mitigated by automated lockout policies and manual incident response intervention.

---

## 🕒 Incident Timeline (90-Minute Window)
- **14:30 (Discovery):** SOC alert triggered by multiple failed login attempts from a non-corporate IP range.
- **14:45 (Escalation):** Attacker successfully submitted the correct password. An **MFA Push challenge** was generated but not approved by the user.
- **15:05 (Persistence):** Unauthorized attempts resumed from a secondary IP, suggesting an attempt to bypass IP-based rate limiting.
- **15:07 (Containment):** Account automatically locked by Okta security policy after reaching the maximum failure threshold.
- **16:00 (Resolution):** Incident resolved after user identity verification and forced credential rotation.

---

## 🔍 Technical Analysis & IOCs
The attack pattern is consistent with **Credential Stuffing**. The attacker likely acquired the user's password from a third-party data breach, as evidenced by the successful password submission at 14:45.

### Indicators of Compromise (IOCs)
| Indicator | Type | Context |
| :--- | :--- | :--- |
| `203.0.113.45` | IPv4 | Primary source; registered to commercial ISP. |
| `198.51.100.27` | IPv4 | Secondary source; utilized after initial lockout timer. |
| `user@example-corp.com` | Target | Affected corporate identity. |

### Authentication Log Analysis
| Timestamp (UTC) | Event Type | Source IP | Result |
| :--- | :--- | :--- | :--- |
| 15:06:21 | `user.session.start` | 203.0.113.45 | **FAILURE (Invalid Password)** |
| 15:06:45 | `user.mfa.challenge` | 203.0.113.45 | **MFA_SENT (No Response)** |
| 15:07:02 | `user.account.lock` | 198.51.100.27 | **LOCKED (Policy Triggered)** |

---

## 🛠️ Response & Remediation
1. **Global Session Invalidation:** Performed an immediate "Clear User Sessions" in the Okta Admin Console to revoke any potential active tokens.
2. **Out-of-Band (OOB) Verification:** Contacted the user via corporate Slack; the user confirmed no login attempts were made personally.
3. **Password Remediation:** Since a valid password was used during the attack, a forced password reset was executed to secure the account.
4. **Impact Assessment:** Audited cross-platform logs (AWS, GitHub) for the same user identity; no successful lateral movement or unauthorized access was detected.

---

## 💡 Lessons Learned & Strategic Recommendations
- **What Worked:** MFA and Account Lockout policies served as critical layers of defense, preventing a full account takeover (ATO).
- **Gaps Identified:** The possession of a valid password by the attacker highlights a need for better monitoring of leaked credentials.
- **Recommendations:** Implement **Conditional Access Policies** to block authentication from high-risk geolocations and prioritize the transition to phishing-resistant MFA (FIDO2/WebAuthn).

### MITRE ATT&CK Mapping
- **T1110.003:** Brute Force (Password Spraying)
- **T1621:** Multi-Factor Authentication Request Generation
- **T1078.004:** Valid Accounts (Cloud Accounts)

---
> **Note:** All indicators and personal data have been sanitized for confidentiality and privacy compliance.
