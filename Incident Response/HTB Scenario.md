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
###### Timeline
```

```

![[Pasted image 20260922172422.png|844]]
![[Pasted image 20260922173748.png|849]]
