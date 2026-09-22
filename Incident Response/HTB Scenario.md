**Victim**: Insight Nexus
Targeted by two threat groups
**Web Consoles**: ManageEngine AD Manager, PHP Client Reports Portal
Entry gained through default admin/admin creds on ManageEngine

#### The Hive 
```
http://<TARGET_IP>:9000
htb-analyst: P3n#31337@LOG
```
#### Mitre Att&ck Mapping

| Tactic            | Technique                                     | ID        | Description                          |
| ----------------- | --------------------------------------------- | --------- | ------------------------------------ |
| Initial Access    | Exploit Public-Facing Application             | T1190     | Confluence CVE Exploited             |
| Execution         | Command and Scripting Interpreter: Powershell | T1059.001 | PowerShell used for payload download |
| Persistence       | Windows Service                               | T1543.003 | Windows Service for Persistence      |
| Credential Access | LSASS Memory Dumping                          | T1003.001 | Extracted Credentials                |
| Lateral Movement  | Remote Desktop Protocol                       | T1021.001 | RDP lateral movement                 |
| Impact            | Data Encrypted for Impact                     | T1486     | LockBit Ransomware                   |
#### Analysis
```
Insight Nexus: market research and data analytics firm in Singapore.
Actors:
	Crimson Fox - credential theft, long-term persistence, data exfiltration
	Silent Jackal - website defacements, proof-of-concept intrusion, low-skill

Initial Access 1: ManageEngine ADManager Plus was left with admin:admin creds after an update.
manage.insightnexus.com
	Created privileged AD accounts
	Found open RDP in the network
	Used GPO's to deploy spyware across multiple endpoints using an MSI package

Initial Access 2: PHP Client reporting portal 
portal.insightnexus.com
	Data already exfiltrated
	Only intermittent callbacks remain
	Event occured before Initial Access 1
	
	Artifacts:
		1. checkme.txt (webserver root) - "SilentJackal was here"
		2. diagnostics_data.zip - packaged stolen client materials from Crimson Fox
		3. C:\Windows\Temp\java-update.msi - creates a scheduled task to run a process to perform spying/exfiltration
```
###### Environment & Assets
```
Public facing:
	manage.insightnexus.com (443)
	portal.insightnexus.com
	
Internal Environment:
	DC: DC01.insight.local
	File Server: FS01.insight.local (\\fs01\projects)
	DB Server: DB01.insight.local
	Workstations: DEV-001 to DEV-120
		DEV-021 has RDP exposed
		
Security:
	Firewall with default logging
	Basic IDS with high false positive rate
	Wazuh agents on MOST Windows hosts
	SIEM ingesting sysmon, Windows Security, web server logs, and firewall logs (with limited retention)
```
###### Timeline - Crimson Fox
```
2025-10-01 03:12:02 - Crimson Fox initial access via ManageEngine default credentials
2025-10-01 03:18:32 - Outbound C2 established using java web vulnerability in ManageEngine - IP: 103.112.60.117
2025-10-02 04:02:11 - Enumerated users and computers from ManageEngine console, created new Domain Administrator account, discovered open RDP on DEV-021
2025-10-04 02:03:12 - 4624 generated for RDP login to DEV-021 as insight\svc_deployer - IP: 103.112.60.117
2025-10-04 02:10:45 - From DEV-021 used powershell script to create a GPO to push an MSI across the domain (java-update.msi)
```
###### Timeline - Silent Jackal
```
Unpatched file upload vulnerability lead to initial access of the PHP portal
No activity in addition to initial access
```
#### Immediate Actions
```
1. Case creation & triage
   SOC created TheHive case "Insight Nexus — ManageEngine Compromise" (Critical - data exfil)
   Linked all related alerts
   Assigned roles Triage Analyst, Forensics Lead, Containment Lead, and Communications Lead
2. Containment - network controls
   Blocked outbound traffic to 103.112.60.117 at firewall and host-based
   Added IDS signature alert for 103.112.60.117
3. Containment - credential & account actions
   Disabled ManageEngine admin account and rotated high-priv credentials
   Restricted access to internal only
   Forced password changes
4. Host Isolation
   Isolated manage.insight.local, DEV-021, and all hosts affected by the GPO MSI
   Suspended scheduled tasks
5. Collect forensic artifacts 
   Collected volatile memory, process lists, registry hives, and disk images
   Preserved copies of the MSI file, data file, and webshells found in management app directories
```
#### MITRE ATT&CK Mapping
```
Reconnaissance:                       Scanning public assets; MITRE T1595 (Active Scanning).
Weaponization / Initial Access:       ManageEngine default credentials (T1078.004 - Valid Accounts), PHP upload exploitation (T1190 - Exploit Public-Facing Application).
Delivery / Exploitation:              Web shell uploads, console command execution; (T1505 - Server Software Component).
Installation / Persistence:           Scheduled tasks, services, GPO-deployed MSI (T1547, T1543, T1069).
Command & Control:                    HTTPS to attacker-controlled IP (T1071.001 - Web Protocols).
Action on Objective / Exfiltration:   Compress and upload project data (T1560/T1041).
```
#### Attack Flow Diagrams - Crimson Fox
![[Pasted image 20260922175400.png]]
![[Pasted image 20260922175619.png]]
![[Pasted image 20260922175916.png]]
![[Pasted image 20260922180242.png]]
#### Attack Flow Diagrams - Silent Jackal
![[Pasted image 20260922180315.png]]

#### Sigma Rules
```
title: External Remote RDP Logon from Public IP
id: 259a9cdf-c4dd-4fa2-b243-2269e5ab18a2
related:
    - id: 78d5cab4-557e-454f-9fb9-a222bd0d5edc
      type: derived
status: test
description: Detects successful logon from public IP address via RDP. This can indicate a publicly-exposed RDP port.
references:
    - https://www.inversecos.com/2020/04/successful-4624-anonymous-logons-to.html
    - https://twitter.com/Purp1eW0lf/status/1616144561965002752
author: Micah Babinski (@micahbabinski), Zach Mathis (@yamatosecurity)
date: 2023-01-19
modified: 2024-03-11
tags:
    - attack.initial-access
    - attack.credential-access
    - attack.t1133
    - attack.t1078
    - attack.t1110
logsource:
    product: windows
    service: security
detection:
    selection:
        EventID: 4624
        LogonType: 10
    filter_main_local_ranges:
        IpAddress|cidr:
            - '::1/128'  # IPv6 loopback
            - '10.0.0.0/8'
            - '127.0.0.0/8'
            - '172.16.0.0/12'
            - '192.168.0.0/16'
            - '169.254.0.0/16'
            - 'fc00::/7'  # IPv6 private addresses
            - 'fe80::/10'  # IPv6 link-local addresses
    filter_main_empty:
        IpAddress: '-'
    condition: selection and not 1 of filter_main_*
falsepositives:
    - Legitimate or intentional inbound connections from public IP addresses on the RDP port.
level: medium
```