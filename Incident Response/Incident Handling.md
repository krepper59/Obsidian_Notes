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