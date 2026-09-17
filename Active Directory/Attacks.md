
| Attack                       | Requirements                                                                                       | Tools                         |
| ---------------------------- | -------------------------------------------------------------------------------------------------- | ----------------------------- |
| ***LLMNR/NBT-NS Poisoning*** | Attacker on same subnet, LLMNR/NBT-NS enabled, victim attempts to connect to non-existent hostname | Responder/Inveigh             |
| ***SMB/NTLM Relay***         | Auth coercion, SMB signing disabled, victim is local admin                                         | Impacket-ntlmrelayx           |
| ***Kerberoasting***          | Domain creds, account has SPN, password is crackable                                               | Rubeus/Impacket-GetUserSPNs   |
| ***ASREPRoasting***          | No kerberos preauth, crackable password                                                            | Rubeus/Impacket-GetNPUsers    |
| ***DCSync***                 | Replicating Directory Changes, Replicating Directory Changes All                                   | Mimikatz/Impacket-secretsdump |
| ***ESC1***                   | Domain creds, active AD CS, published template with "Enroll" rights, template allows client auth   |                               |
