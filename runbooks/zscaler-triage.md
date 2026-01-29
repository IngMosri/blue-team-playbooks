# Zscaler Alert Triage Runbook

## Objective
Provide a step-by-step workflow to analyze web proxy alerts.

## Step 1: Validate Alert Context
- Source IP / User ID (sanitized)
- URL category
- Risk score

## Step 2: Threat Intelligence Check
- VirusTotal
- Talos
- AbuseIPDB

## Step 3: Endpoint Validation
- CrowdStrike / Defender status
- Patch compliance
- Recent user activity

## Step 4: Classification
- False Positive
- Suspicious
- Confirmed Incident

## Step 5: Escalation
- SOC L2 if confirmed malicious
- IT Endpoint team if remediation required
