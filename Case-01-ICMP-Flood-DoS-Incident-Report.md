Case 01 — ICMP Flood DoS Attack: Incident Report Analysis
Analyst: Kowsalya  
Date: May 2026  
Framework: NIST Cybersecurity Framework (CSF)  
Category: Denial of Service / Network Attack  
Severity: High
---
Scenario Overview
A multimedia company suffered a two-hour network outage caused by an ICMP Flood
(Denial of Service) attack. The attacker exploited an unconfigured firewall to
flood the internal network with ICMP ping requests, overwhelming the system and
paralysing internal services. Client-facing web services were significantly
disrupted during this period.
---
Incident Summary
Field	Details
Attack Type	ICMP Flood — Denial of Service (DoS)
Root Cause	Unconfigured firewall with no ICMP rate limiting
Impact	Internal services paralysed for 2 hours
Affected Systems	Network infrastructure, client web services
Business Impact	Marketing operations and client services disrupted
---
NIST CSF Analysis
1. Identify
Goal: Understand the environment and what needs protection.
Asset Inventory: Catalogued all network devices, including the unconfigured
firewalls that allowed the ICMP flood to pass through unchecked
Vulnerability Assessment: Conducted a formal audit to identify other
out-of-the-box or default configurations across the network infrastructure
Risk Assessment: Recognised that as a multimedia company, high network
availability is critical for client web services and marketing operations —
downtime directly impacts revenue
---
2. Protect
Goal: Implement safeguards to prevent or limit the impact of an incident.
Firewall Hardening: Implemented rate limiting rules to restrict the volume
of incoming ICMP packets from any single source
Access Control: Deployed Source IP address verification to prevent spoofing,
ensuring traffic originates from legitimate locations
Technical Safeguards: Deployed IDS/IPS (Intrusion Detection and Prevention
System) configured to automatically drop packets matching known DoS attack
signatures
---
3. Detect
Goal: Identify cybersecurity events quickly.
Continuous Monitoring: Deployed network monitoring software to establish a
baseline of normal traffic behaviour — deviations now trigger alerts
Alerting Mechanisms: Configured thresholds so that if ICMP traffic exceeds
a defined percentage of total bandwidth, the security team receives an immediate
notification
Log Analysis: Centralised firewall logs into a SIEM for real-time visibility
into incoming traffic spikes and anomalies
---
4. Respond
Goal: Take action once an incident is detected.
Containment: Blocked the ICMP flood immediately and took non-critical
services offline to preserve bandwidth for critical systems
Analysis: Investigated the source IP addresses of the ICMP packets to
determine whether the attack was targeted or an opportunistic scan
Mitigation: Applied lessons learned from the unconfigured firewall to all
other network entry points to prevent repeat incidents
---
5. Recover
Goal: Restore services and document lessons learned.
Service Restoration: Brought non-critical network services back online in
a prioritised sequence — critical services first, then secondary systems
Communication: Updated stakeholders including internal teams and small
business clients on the resolution and the hardening steps taken
Post-Incident Review: Finalised this incident report and updated the
company's disaster recovery and business continuity plans
---
MITRE ATT&CK Mapping
Tactic	Technique	ID
Impact	Network Denial of Service — ICMP Flood	T1498.001
Defense Evasion	Firewall misconfiguration exploitation	T1562.004
---
Key Vulnerabilities Identified
Unconfigured firewall — No ICMP rate limiting or packet inspection rules
No traffic baseline — No monitoring to detect abnormal traffic volume
No IDS/IPS — Malicious traffic was not automatically detected or blocked
No alerting thresholds — Security team was not notified in real time
---
Recommendations
Priority	Recommendation	Control Type
Critical	Implement ICMP rate limiting on all firewalls	Technical
Critical	Deploy IDS/IPS with DoS signature detection	Technical
High	Centralise logs in SIEM for real-time monitoring	Technical
High	Define and monitor network traffic baselines	Operational
Medium	Conduct quarterly firewall configuration audits	Managerial
Medium	Update business continuity plan with DoS scenarios	Managerial
---
Lessons Learned
Default firewall configurations are never sufficient for production environments
ICMP traffic must always be rate-limited — it is a common DoS vector
A SIEM with real-time alerting would have reduced the 2-hour outage
significantly — early detection is everything in incident response
The NIST CSF provides a clear, repeatable structure for responding to and
recovering from incidents systematically
---
Tools Referenced
Network monitoring software (IDS/IPS)
SIEM (centralised log management)
Firewall with rate limiting capability
NIST Cybersecurity Framework
MITRE ATT&CK Framework
