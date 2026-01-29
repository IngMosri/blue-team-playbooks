# Okta Unauthorized Access Attempt Case Study

## Scenario Overview
Multiple failed authentication attempts were detected against a corporate account protected by Okta MFA. The identity platform automatically locked the account according to security policy.

## Detection Source
- Identity Provider: Okta
- Alert Type: Unauthorized authentication attempts
- Security Control: MFA enforcement and account lockout policy

## Initial Triage
- Multiple failed login attempts detected in a short time window.
- Geo-location indicated access attempts from foreign IP ranges.
- Account was automatically locked by Okta security policy.

### Observed Source IPs (Sanitized)
The following source IP addresses were observed during the unauthorized authentication attempts:

- 203.0.113.45  
- 198.51.100.27  

Geo-location analysis indicated access attempts from outside the corporate region, suggesting possible VPN or proxy usage.

> Note: All indicators of compromise (IOCs) have been sanitized to comply with corporate confidentiality and privacy policies.

---

## Investigation Steps

### 1. Log Review
- Reviewed Okta authentication logs for failed login attempts.
- Confirmed no successful authentication events occurred.

### 2. IP Reputation Analysis
- Validated IP reputation using threat intelligence sources.
- No confirmed malicious indicators were found in public threat intelligence feeds.
- Traffic patterns suggested anonymization via VPN or proxy infrastructure.

### 3. Account Security Validation
- Verified MFA enforcement was active.
- Confirmed no session hijacking or privilege escalation occurred.

### 4. Containment Actions
- Forced global sign-out for the affected account.
- Maintained account lock pending investigation.
- Coordinated password reset with IT and security teams.

---

## Simulated Authentication Log Sample

| Timestamp (UTC) | User | Source IP | Result |
|----------------|------|-----------|--------|
| 2026-01-23 15:06:21 | user@example-corp.com | 203.0.113.45 | Failed |
| 2026-01-23 15:06:45 | user@example-corp.com | 203.0.113.45 | MFA Challenge |
| 2026-01-23 15:07:02 | user@example-corp.com | 198.51.100.27 | Account Locked |

---

## Findings
No account compromise was detected. The security controls successfully prevented unauthorized access. The event was classified as an **Unauthorized Access Attempt**.

---

## Lessons Learned
- MFA effectively mitigated credential-based attacks.
- Authentication log monitoring is critical for early detection.
- Lockout policies reduce brute-force attack impact.
- Cross-team coordination improves identity incident response.

---

## MITRE ATT&CK Mapping
- T1110: Brute Force  
- T1078: Valid Accounts  

---

## Recommendations
- Implement phishing-resistant MFA (FIDO2/WebAuthn)
- Apply conditional access policies for risky geolocations
- Enable automated alerting for abnormal login patterns
- Conduct periodic user security awareness training
