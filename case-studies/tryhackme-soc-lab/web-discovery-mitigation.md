Incident Report: Web Enumeration and Reconnaissance Detection (THM Lab)
Executive Summary
This report analyzes a Web Discovery Attack detected during a SOC simulation. An external threat actor attempted to identify hidden administrative directories on a financial platform. The investigation led to the identification of a malicious Russian-based IP address and the subsequent implementation of automated blocks to prevent further reconnaissance.

🕒 Incident Timeline (UTC)
10:21:39: Attack detected originating from source IP 32.122.195.63.

10:35 - 10:39: Attacker generated multiple 404 (Not Found) and 403 (Forbidden) errors while targeting sensitive paths like /admin, /administrator, and /wp-admin.

10:45: Threat Intelligence confirmed the source IP as malicious, located in Moscow, Russia.

10:50: Incident mitigated via IP blacklisting and WAF rule updates.

🔍 Technical Analysis
The attacker used automated tools to perform Directory Enumeration. By analyzing the web server response codes, we identified a clear reconnaissance pattern.

Evidence: URL Discovery Attempts
The attacker systematically probed the following endpoints:

https://fakebank.com/admin (404)

https://fakebank.com/administrator (403)

https://fakebank.com/login (404)

The high frequency of 404 errors in a short window is a primary indicator of automated "fuzzing" or directory bursting tools (e.g., Dirb or Gobuster).

Figure 1: Log analysis showing automated GET requests and server response codes.

Threat Intelligence
Source IP: 32.122.195.63

Geolocation: Moscow, Russia (AS12345)

Reputation: Flagged as Malicious with 47 previously reported incidents.

Figure 2: Threat Intelligence dashboard confirming the malicious origin of the traffic.

🛠️ Response and Mitigation
Based on the "Recommended Actions" from the SOC dashboard, the following steps were taken to secure the environment:

IP Blocking: Immediate 24-hour block was applied to IP 32.122.195.63 to terminate the reconnaissance phase.

Rate Limiting: Implemented request thresholds on all administrative endpoints to mitigate future automated scanning.

WAF Update: Deployed new Web Application Firewall rules to detect and drop traffic exhibiting similar directory-bursting signatures.

Figure 3: Deployment of security actions to block the threat actor.

💡 Lessons Learned
The Importance of 404 Monitoring: Monitoring spikes in 404/403 errors is crucial for early detection of the reconnaissance phase before an attacker finds a viable entry point.

Geographic Risk: While TLD/Geolocation blocking isn't always feasible, correlating it with reputation data (47 previous incidents) provides high-confidence evidence for blocking.

Proactive Defense: Moving from detection to automated mitigation (WAF/Rate Limiting) significantly reduces the attack surface.

MITRE ATT&CK Mapping
T1595.002: Active Scanning: Vulnerability Scanning.

T1071.001: Application Layer Protocol: Web Protocols.
