## Process
```
1. Initital Detection & Acknowledgment - automated or analyst alerts/detections
2. Preliminary Analysis - scope and ramifications established
3. Incident Logging - JIRA/TheHive
4. Notification of Relevant Parties - internal and external stakeholders notified
5. Detailed Investigation & Reporting - technical analysis and findings compilation
6. Final Report Creation - detailing findings from the investigation
7. Feedback Loop - revisiting incidents to improve processes
```
## Incident Report
#### Executive Summary
```
Succinct overview, key findings, immediate actions executed, and the impact on stakeholders

1. Incident ID - Unique identifier for the incident
2. Incident Overview - Breif summary of events
3. Key Findings - root cause, CVE's, data exfiltrated, etc.
4. Immediate Actions Taken - system isolation, third parties involved, system updates, etc.
5. Stakeholder Impact - assess time/money/risk associated with stakeholders
```
#### Technical Analysis
```
Technical deep-dive of events of the incident

1. Affected Systems & Data - state impact on specific systems/networks and data
2. Evidence Sources & Analysis - state sources of events and how they were analyzed
3. IoC's - state indicators and map to known threats
4. Root Cause Analysis - elaborate on the cause of the incident
5. Technical Timeline:
	- Reconnaissance
	- Initial Compromise
	- C2 Communications
	- Enumeration
	- Lateral Movement
	- Data Access & Exfiltration
	- Malware Deployment or Activity (including Process Injection and Persistence)
	- Containment Times
	- Eradication Times
	- Recovery Times
6. Nature of the Attack - deep-dive into the typeof attack
```
#### Impact Analysis
```
Provide an evaluation of the adverse effects that the incident had on the organization's data, operations, and reputation. This analysis aims to quantify and qualify the extent of the damage caused by the incident, identifying which systems, processes, or data sets have been compromised. It also assesses the potential business implications, such as financial loss, regulatory penalties, and reputational damage.
```
#### Response & Recovery Analysis
```
Outline the specific actions taken to contain the security incident, eradicate the threat, and restore normal operations.
```
###### Immediate Response Actions
```
Revocation of Access:
	1. Identification of compromised accounts/systems
	2. Timeframe
	3. Method of Revocation
	4. Impact

Containment Strategy:
	1. Short-term Containment - Immediate actions taken
	2. Long-term Containment - Strategic measures for long-term isolation
	3. Effectiveness - usefulness of containment strategies
```
###### Eradication Measures
```
Malware Removal:
	1. Identification - details how malware/malicious code was identified
	2. Removal Techniques - specify tools and methods
	3. Verification - steps taken to ensure removal 
	   
System Patching:
	1. Vulnerability Identification - how vulns were discovered
	2. Patch Management - details of patching process
	3. Fallback Procedures - steps to revert patches in case of issues
```
###### Recovery Steps:
```
Data Restoration:
	1. Backup Validation Procedures
	2. Restoration Process
	3. Data Integrity Checks & Methods

System Validation:
	1. Security Measure/Actions
	2. Operational Checks to confirm system operation
```
###### Post-Incident Actions
```
Monitoring:
	1. Enhanced Monitoring Plans
	2. Tools and Technologies

Lessons Learned:
	1. Gap Analysis - what security measures failed and why
	2. Recommendations - actionable items based on lessons learned
	3. Future Strategy - policy/architecture/training changes to prevent future events
```
#### Diagrams
```

```