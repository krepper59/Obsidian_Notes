NIST Computer Security Incident Handling Guide: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r3.pdf
Example Incident Report: https://thedfirreport.com/2025/02/24/confluence-exploit-leads-to-lockbit-ransomware/
DFIR Labs Incident Reports: https://thedfirreport.com/
Global Incident Response Report: https://www.paloaltonetworks.com/engage/unit42-2025-global-incident-response-report

### Cyber Kill Chain

| Stage     | Description                                                                        |
| --------- | ---------------------------------------------------------------------------------- |
| Recon     | Attacker chooses target and gathers information, passive and active reconnaissance |
| Weaponize | Malware for initial access is embedded in payload                                  |
| Deliver   | Exploit/Payload is delivered to victim machine or network                          |
| Exploit   | Payload is triggered or executed                                                   |
| Install   | Droppers, Backdoors, or Rootkits establish connection to the victim                |
| C&C       | Remote access to the host or network is established                                |
| Action    | Actions performed after access is established                                      |
### Mitre Att&ck
https://attack.mitre.org/
**Columns** - Adversary Goals
**Cells** - Techniques Used
**Tactic** - High level adversary objective (Initial Access, Persistence, Privilege Escalation)
**Technique** - Specific method used to achieve a tactic (T1105 Ingress Tool Transfer, T1021 Remote Services)
**Sub-Technique** - Children of techniques for specific implementations (T1003.001 OS Credentials: LSASS Memory)
### Process

| Stages                               | Description                                                                         |
| ------------------------------------ | ----------------------------------------------------------------------------------- |
| Preparation                          | Establishment of incident handling capability and security event protection ability |
| Detection & Analysis                 |                                                                                     |
| Containment, Eradication, & Recovery |                                                                                     |
| Post-Incident Activity               |                                                                                     |
#### Preparation
###### Policies and Documentation
```
1. Contact information and roles of the incident handling team members.
2. Contact information for the legal and compliance department, management team, IT support, communications and media relations department, law enforcement, internet service providers, facility management, and external incident response team.
3. Incident response policy, plan, and procedures.
4. Incident information sharing policy and procedures.
5. Baselines of systems and networks, out of a golden image and a clean state environment
6. Network diagrams
7. Organization-wide asset management database
8. User accounts with excessive privileges that can be used on-demand by the team when necessary (also for business-critical systems, which are handled with the skills needed to administer that specific system). These user accounts are normally enabled when an incident is confirmed during the initial investigation and then disabled once it is over. A mandatory password reset is also performed when disabling the users.
9. Ability to acquire hardware, software, or an external resource without a complete procurement process (urgent purchase of up to a certain amount). The last thing you need during an incident is to wait for weeks for the approval of a $500 tool.
10. Forensic/Investigative cheat sheets.
```
###### Tools (Software & Hardware 'Jump Bag')
```
1. An additional laptop or a forensic workstation for each incident handling team member to preserve disk images and log files, perform data analysis, and investigate without any restrictions (we know malware will be tested here, so tools such as antivirus should be disabled). These devices should be handled appropriately and not in a way that introduces risks to the organization.
2. Digital forensic image acquisition and analysis tools.
3. Memory capture and analysis tools.
4. Live response capture and analysis tools.
5. Log analysis tools.
6. Network capture and analysis tools.
7. Network cables and switches.
8. Write blockers.
9. Hard drives for forensic imaging.
10. Power cables.
11. Screwdrivers, tweezers, and other relevant tools to repair or disassemble hardware devices if needed.
12. Indicator of Compromise (IOC) creator and the ability to search for IOCs across the organization.
13. Chain of custody forms.
14. Encryption software.
15. Ticket tracking system.
16. Secure facility for storage and investigation.
17. Incident handling system independent of your organization's infrastructure.
```
###### DMARC
```
Email protection to reject emails pretending to originate from our organization (spoofing).
```

###### Endpoint Hardening (EDR)
```
1. Disable LLMNR/NetBIOS.
2. Implement LAPS and remove administrative privileges from regular users.
3. Disable or configure PowerShell in "ConstrainedLanguage" mode.
4. Enable Attack Surface Reduction (ASR) rules if using Microsoft Defender.
5. Implement whitelisting. We know this is nearly impossible to implement. Consider at least blocking execution from user-writable folders (Downloads, Desktop, AppData, etc.). These are the locations where exploits and malicious payloads will initially find themselves. Remember to also block script types such as .hta, .vbs, .cmd, .bat, .js, and similar. We need to pay attention to LOLBin files while implementing whitelisting. Do not overlook them; they are really used in the wild as initial access to bypass whitelisting.
6. Utilize host-based firewalls. As a bare minimum, block workstation-to-workstation communication and block outbound traffic to LOLBins.
7. Deploy an EDR product. At this point in time, AMSI provides great visibility into obfuscated scripts for antimalware products to inspect the content before it gets executed. It is highly recommended that we only choose products that integrate with AMSI.
```
###### Network Protection
```
Network Segmentation - isolating critical systems, preventing public accessibility without the use of a DMZ
IDS/IPS (Intrusion Detection System/Intrusion Prevention System) - performs SSL/TLS interception to identify malicious traffic
802.1x - port based access control to ensure only org machines can access the network
```
###### Privilege Identity Management/MFA/Passwords
```
1. Implement strong passwords across the domain
2. Utilize MFA, especially for any administrative applications and devices
```
###### Vulnerability Scannning
```
Should be continuous and ongoing addressing all critical and high while isolating systems that cannot be patched.
```
###### User Awareness Training
```
Train users on suspicious behavior/activity and methods of reporting the activity
```
###### Active Directory Security Assessment
```
View AD from an attacker perspective to see vulnerabilities in configurations and attack paths that attackers could leverage.
```
###### Purple Team Exercises
```
Collaboration between Red Team and Blue Team to see visibility into monitoring and alerting on attacks and techniques.
```
#### Detection & Analysis
###### Create Levels of Detection
```
1. Detection at the network perimeter (using firewalls, internet-facing network intrusion detection/prevention systems, demilitarized zone, etc.).
2. Detection at the internal network level (using local firewalls, host intrusion detection/prevention systems, etc.).
3. Detection at the endpoint level (using antivirus systems, endpoint detection & response systems, etc.).
4. Detection at the application level (using application logs, service logs, etc.).
```
###### Initial Investigation
```
1. Date/Time when the incident was reported. Additionally, who detected the incident and/or who reported it?
2. How was the incident detected?
3. What was the incident? Phishing? System unavailability? etc.
4. Assemble a list of impacted systems (if relevant).
5. Document who has accessed the impacted systems and what actions have been taken. Make a note of whether this is an ongoing incident or if the suspicious activity has been stopped.
6. Physical location, operating systems, IP addresses and hostnames, system owner, system's purpose, current state of the system.
7. List of IP addresses, if malware is involved, time and date of detection, type of malware, systems impacted, export of malicious files with forensic information on them (such as hashes, copies of the files, etc.).
```
###### Timeline
```
1. Date
2. Time of Event
3. Hostname
4. Event Description
5. Data Source
```
###### Incident Severity & Questions
```
1. What is the exploitation impact?
2. What are the exploitation requirements?
3. Can any business-critical systems be affected by the incident?
4. Are there any suggested remediation steps?
5. How many systems have been impacted?
6. Is the exploit being used in the wild?
7. Does the exploit have any worm-like capabilities?
```
###### The Investigation
```
Iterative Process:
1. Creation and usage of indicators of compromise (IOCs).
2. Identification of new leads and impacted systems.
3. Data collection and analysis from the new leads and impacted systems.
```
##### IOC Creation & Usage
```
IOC - Sign that an incident has occurred (IP, hash, file names)
1. OpenIOC
2. YARA

IOC Editor: Tool to create and edit IOC's

STIX (Structured Threat Information eXpression):
Open-source machine-readable language and serialization format in JSON to exchange Cyber Threat Intelligence (CTI)
```
###### Collect & Preserve State
```
Note volatility of data, perform live analysis where applicable to maintain in-memory artifacts.
Keep track of chain of custody for incidents where data needs to be court-admissible.
```
###### Ai Threat Detection
```
Elastic Security Attack Discovery - analyzes events, summarizes, and clusters relevant alerts

1. Automated Triage & Alert Prioritization
2. Incident Correlation & Timeline Reconstruction
3. Automated Response Playbooks
4. AI Assistance in Post-Incident Analysis & Learning
```
#### Containment, Eradication, & Recovery
Prevent the ongoing incident from causing more damage, prevent further spread and eliminate current threat.
###### Containment
```
Short-Term Containment: actions taken leave minimal footprint (Ex: isolating system in VLAN, pulling network cable, modifying attackers C2 to redirect to a system we own.) and backups are created.
Long-Term Containment: persistent actions and changes (Ex: password changes, firewalls, EDR, patches, shutdowns.)
```
###### Eradication
```
Eliminate the root cause of the incident and any remaining artifacts from the incident (Ex: removing malware, rebuilding and restoring affected systems, system hardening.)
```
###### Recovery
```
Bring systems and the network back to normal operation with heavy monitoring for further activity, looking for:
1. Unusual logons (e.g., user or service accounts that have never logged-in there before).
2. Unusual processes.
3. Changes to the registry in locations that are usually modified by malware.
```
#### Post-Incident Activity

### Activities

| Actvity       | Description                                                               |
| ------------- | ------------------------------------------------------------------------- |
| Investigating | Detecting and investigating a cyber incident                              |
| Recovering    | Developing and implementing a plan to return to normal business operation |
