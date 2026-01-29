# Web Proxy Alert Investigation Playbook

## Objective
Validate anomalous web traffic alerts and identify potential security incidents while respecting corporate privacy policies.

## Alert Source
- Secure Web Gateway (Zscaler)
- SIEM correlation rules

## Investigation Steps

1. Validate domain reputation
   - VirusTotal
   - Talos Intelligence
   - AbuseIPDB

2. Review web category and policy classification
3. Analyze alert volume anomalies
4. Validate endpoint posture
   - EDR health status
   - Patch compliance
   - AV signature status

## Decision Matrix

| Condition | Action |
|-----------|--------|
| Confirmed malicious | Escalate to SOC L2 |
| Suspicious | Monitor and collect additional telemetry |
| False positive | Document and close alert |

## User Communication Template
"Automated security alerts were generated from your endpoint. We are validating system health. Please confirm if you noticed abnormal behavior."

## Documentation Requirements
- Alert ID
- Timestamp
- Analyst actions
- Final outcome

## MITRE ATT&CK Mapping
- T1071: Application Layer Protocol
- T1046: Network Service Scanning
