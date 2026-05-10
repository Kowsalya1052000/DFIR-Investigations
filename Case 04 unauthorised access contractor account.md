Case 04 — Unauthorised Access: Contractor Account Exploitation
---
Analyst: Kowsalya  
Date: May 2026  
Category: Access Control / Identity & Authentication  
Framework: NIST SP 800-53 AC-6 (Least Privilege)  
Severity: Critical
---
Scenario Overview
A security investigation identified that a former contractor's account was
used to access sensitive payroll systems — four years after the contractor's
engagement ended. This case demonstrates the critical importance of access
control lifecycle management and the principle of least privilege.
---
Incident Summary
Field	Details
Threat Actor	Robert Taylor Jr. (former contractor)
Incident Time	8:29:57 AM
Source IP	152.207.255.255
Access Level	Administrator
Systems Accessed	Payroll systems
Contract End Date	2019
Unauthorised Access Date	2023
Gap	4 years of active account after contract ended
---
Investigation Analysis
Authentication & Authorisation Assessment
Area	Notes	Issues Identified	Recommendations
Who caused the incident?	Robert Taylor Jr. — former contractor	Account remained active 4 years after contract ended	Implement automatic account expiry for all contractors
When did it occur?	8:29:57 AM	Outside normal business hours — not flagged	Set up alerting for after-hours access to sensitive systems
What device was used?	IP: 152.207.255.255	External IP — access from outside corporate network	Restrict payroll access to internal network or VPN only
Access level	Administrator	Contractor had admin access — far beyond what was needed	Apply least privilege — contractors should have minimal access only
Account status	Active in 2023 despite 2019 contract end	No account lifecycle management in place	Automate account deactivation tied to contract end dates
---
Root Cause
Primary cause: Failure to implement account lifecycle management.
The contractor's account was never deactivated when the contract ended in
2019. This gave the threat actor a valid, active set of credentials with
full administrator privileges — an open door that remained unlocked for
four years.
Contributing factors:
No automatic account expiry policy for contractor accounts
No periodic access review or audit
No alerting on dormant accounts becoming active after long periods
Excessive privilege level assigned to a contractor role
---
Access Control Failures Mapped to NIST SP 800-53 AC-6
AC-6 — Principle of Least Privilege: Users should only have the minimum
access required to perform their role.
AC-6 Requirement	Status in This Case
Restrict access based on user role	❌ Failed — contractor had admin access
Automatically revoke access after a period	❌ Failed — account active 4 years after contract end
Keep activity logs of provisioned accounts	❌ Failed — no alerts on dormant account reactivation
Regularly audit user privileges	❌ Failed — no access review process in place
---
Evidence
Evidence	Value	Significance
Threat actor	Robert Taylor Jr.	Identified former contractor
Login timestamp	8:29:57 AM	Exact time of unauthorised access
Source IP	152.207.255.255	External IP — not corporate network
Access level	Administrator	Excessive privilege for contractor role
Account age	Active since before 2019	Never deactivated after contract ended
---
MITRE ATT&CK Mapping
Tactic	Technique	ID
Initial Access	Valid Accounts — Local Accounts	T1078.003
Persistence	Account Manipulation	T1098
Defence Evasion	Use of Valid Credentials	T1078
---
Recommendations
Priority	Recommendation	Control Type
Critical	Immediately deactivate all contractor accounts upon contract end	Technical + Operational
Critical	Audit all current contractor accounts — identify any dormant active accounts	Managerial
High	Enable Multi-Factor Authentication (MFA) on all accounts especially admin	Technical
High	Restrict payroll system access to internal network or VPN only	Technical
High	Implement automatic account expiry — 30 days maximum for contractors	Technical
Medium	Set up alerts for dormant accounts that suddenly become active	Technical
Medium	Apply least privilege — contractors should never have admin access	Operational
Medium	Conduct quarterly access reviews for all privileged accounts	Managerial
---
Timeline of Events
Time	Event
2019	Robert Taylor Jr. contract ends
2019–2023	Account remains active — undetected, unreviewed
2023 8:29:57 AM	Unauthorised login to payroll systems from external IP
Post-incident	Security investigation initiated
---
Lessons Learned
Access control is a lifecycle, not a one-time setup. Accounts must be
managed from creation through to deactivation — contractor offboarding
must include immediate account deactivation
Least privilege is non-negotiable. A contractor should never hold
admin privileges — this dramatically increases blast radius if credentials
are compromised
Dormant accounts are a significant threat vector. An account unused
for years suddenly becoming active is a high-confidence indicator of
compromise — this should always trigger an immediate alert
MFA would have prevented this. Even with valid credentials, MFA adds
a second layer that the threat actor would not have been able to bypass
without access to the account owner's device
---
Frameworks Referenced
NIST SP 800-53 AC-6 — Least Privilege
NIST Cybersecurity Framework — Identify, Protect, Detect
MITRE ATT&CK — T1078 Valid Accounts
