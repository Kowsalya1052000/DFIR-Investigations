# Case 02 — DNS Port Unreachable: Network Traffic Analysis
**Analyst:** Kowsalya  
**Date:** May 2026  
**Tool Used:** tcpdump  
**Category:** Network Forensics / DNS Investigation  
**Severity:** High  
---
## Scenario Overview
Customers reported being unable to access the website `www.yummyrecipesforme.com`, 
receiving the error message "destination port unreachable." The security team was 
tasked with investigating the root cause using network traffic analysis tools.
---
## Incident Summary

| Field | Details |
|---|---|
| **Reported By** | Customers unable to access website |
| **Error Message** | "destination port unreachable" |
| **Tool Used** | tcpdump (packet analyser) |
| **Protocol Affected** | DNS (UDP Port 53) |
| **Suspected Cause** | DNS server down or Port 53 blocked by firewall |
---
## Investigation Process
### Step 1 — Initial Report
Customers contacted the organisation reporting they could not load 
`www.yummyrecipesforme.com`. The error displayed was "destination port 
unreachable" after waiting for the page to load.
### Step 2 — Reproducing the Issue
The analyst visited the website independently and confirmed the same error 
was present. This ruled out isolated user-side issues and confirmed 
an infrastructure-level problem.
### Step 3 — Network Traffic Capture with tcpdump
Launched `tcpdump` to capture live network traffic while attempting to 
load the webpage. The packet capture revealed the following sequence:
```
Browser → DNS Server : UDP packet to Port 53 (DNS query for yummyrecipesforme.com)
DNS Server → Browser : ICMP error — "udp port 53 unreachable"
```
This sequence repeated consistently across multiple attempts.
### Step 4 — Protocol Analysis

| Protocol | Role | Observation |
|---|---|---|
| **UDP** | Carries DNS queries from browser to DNS server | Packets sent successfully |
| **DNS** | Resolves domain name to IP address | Resolution failed — port 53 unreachable |
| **ICMP** | Returns error messages when a port is unreachable | Returned "udp port 53 unreachable" |
**Key finding:** Port 53 is the standard port for DNS traffic. An "unreachable" 
error on Port 53 means the DNS server is not accepting connections — either 
because the service is down or the port is being blocked.
---
## Root Cause Analysis
### Primary Finding
`UDP Port 53 unreachable` — The DNS server is not responding to name 
resolution requests.
### Possible Causes

| Cause | Likelihood | Description |
|---|---|---|
| DNS server DoS attack | High | Attacker flooded DNS server causing it to crash |
| DNS server misconfiguration | Medium | Incorrect configuration causing service failure |
| Firewall blocking Port 53 | Medium | Firewall rule change blocking DNS traffic |
| DNS server hardware failure | Low | Physical or VM-level server failure |
---
## Evidence
| Evidence | Value | Significance |
|---|---|---|
| Error message | "udp port 53 unreachable" | Confirms DNS port not accessible |
| Protocol used | UDP | Standard for DNS queries — confirms DNS attempt |
| ICMP response | Error packet returned | Server is alive but port is blocked/down |
| Port number | 53 | Universally assigned to DNS service |
---
## Timeline of Events
| Time | Event |
|---|---|
| 1:24 PM | First customer reports website unreachable |
| 1:24 PM | Analyst confirms error independently |
| 1:30 PM | tcpdump packet capture initiated |
| 1:32 PM | UDP Port 53 unreachable error identified in logs |
| Ongoing | Investigation escalated to senior security engineers |

---

## Next Steps

1. Verify whether the DNS server itself is down or just Port 53 is blocked
2. Check firewall rules for any recent changes blocking Port 53
3. Check DNS server logs for signs of a DoS attack or service crash
4. If DNS server is confirmed down — initiate failover to backup DNS server
5. If firewall misconfiguration — roll back the rule change immediately
---
## MITRE ATT&CK Mapping
| Tactic | Technique | ID |
|---|---|---|
| Impact | Network Denial of Service | T1498 |
| Discovery | Network Service Scanning | T1046 |
---
## Lessons Learned
- DNS is critical infrastructure — any disruption immediately impacts all 
  users trying to access web services
- tcpdump is a powerful first-response tool for quickly identifying which 
  protocol and port is failing
- ICMP error messages provide valuable clues — "port unreachable" specifically 
  tells us the host is reachable but the service on that port is not
- Backup DNS servers should always be configured for failover
---

## Tools Used

- **tcpdump** — network packet analyser for traffic capture
- **ICMP analysis** — interpreting error response messages
- **DNS protocol knowledge** — understanding port 53 and UDP query structure
