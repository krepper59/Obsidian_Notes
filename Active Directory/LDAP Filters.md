### LDAP Filters
```
#Find all workstations
(objectCategory=computer)

#Find all domain controllers
(&(objectCategory=Computer)(userAccountControl:1.2.840.113556.1.4.803:=8192))

#Find all users
(&(objectCategory=person)(objectClass=user))

#Find all users with descriptions
(&(objectCategory=user)(description=*))

#Search all groups
(objectClass=group)

#Find disabled accounts
(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))

#Find all groups with <USER>
(member:1.2.840.113556.1.4.1941:=CN=<USER>,OU=<OU>,OU=<OU>,OU=<OU>,DC=<DOMAIN>,DC=<DOMAIN>)

#Find all groups with <USER> recursively
Get-ADGroup -Filter 'member -RecursiveMatch "CN=<USER>,OU=<OU>,OU=<OU>,OU=<OU>,DC=<DOMAIN>,DC=<DOMAIN>"' | select name
Get-ADGroup -LDAPFilter '(member:1.2.840.113556.1.4.1941:=CN=<USER>,OU=<OU>,OU=<OU>,OU=<OU>,DC=<DOMAIN>,DC=<DOMAIN>)' |select Name

#Find all users and computers trusted for deleg
(userAccountControl:1.2.840.113556.1.4.803:=524288)

#Find all users with blank password
(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))(adminCount=1)

#Links
https://ldapwiki.com/wiki/Wiki.jsp?page=Active%20Directory%20Computer%20Related%20LDAP%20Query
https://ldapwiki.com/wiki/Wiki.jsp?page=Active%20Directory%20User%20Related%20Searches
https://ldapwiki.com/wiki/Wiki.jsp?page=Active%20Directory%20Group%20Related%20Searches
```
### LDAP Queries
```
Get-ADObject -LDAPFilter '<FILTER>' | select name
Get-ADObject -LDAPFilter '<FILTER>' -Properties * | select samaccountname,useraccountcontrol

#Filter count
(Get-ADObject -LDAPFilter '<FILTER>').Count
```
### AD Filters
```
Filter			Meaning
-eq				Equal to
-le				Less than or equal to
-ge				Greater than or equal to
-ne				Not equal to
-lt				Less than
-gt				Greater than
-approx			Approximately equal to
-bor			Bitwise OR
-band			Bitwise AND
-recursivematch	Recursive match
-like			Like
-notlike		Not like
-and			Boolean AND
-or				Boolean OR
-not			Boolean NOT

AND Operation:
	One criteria: (& (..C1..) (..C2..))
	More than two criteria: (& (..C1..) (..C2..) (..C3..))
OR Operation:
	One criteria: (| (..C1..) (..C2..))
	More than two criteria: (| (..C1..) (..C2..) (..C3..))
We can also have nested operations, for example "(|(& (..C1..) (..C2..))(& (..C3..) (..C4..)))" translates to "(C1 AND C2) OR (C3 AND C4)".

#Examples
Get-ADComputer  -Filter "DNSHostName -like 'SQL*'"
Get-ADGroup -Filter "adminCount -eq 1" | select Name
Get-ADUser -Filter {adminCount -eq '1' -and DoesNotRequirePreAuth -eq 'True'}
Get-ADUser -Filter "adminCount -eq '1'" -Properties * | where servicePrincipalName -ne $null | select SamAccountName,MemberOf,ServicePrincipalName | fl
Get-ADUser -Filter {DoesNotRequirePreAuth -eq 'True'}
Get-ADComputer -Identity "WS01" -Properties *
Get-ADUser -Filter * -Properties *
```
### SearchBase and SearchScope
```
#SEARCHBASE
specifies an Active Directory path to search under
allows us to define how deep into the OU hierarchy we would like to search

#SEARCHSCOPE
Base			0		OU itself
OneLevel		1		Inside the OU
SubTree			2		OU and everything in it
```
### UAC
```
#Conversion Script
https://academy.hackthebox.com/storage/resources/Convert-UserAccountControlValues.zip?_gl=1*1t1eutk*_gcl_au*MTI1MjM5NjYzMC4xNzgyMTMwMjI3
.\Convert-UserAccountControlValues.ps1

#Get UAC attributes
Get-ADUser -Filter {adminCount -gt 0} -Properties admincount,useraccountcontrol | select Name,useraccountcontrol
#PowerView
Get-DomainUser * -AdminCount | select samaccountname,useraccountcontrol

#pwdneverexpires
dsquery user "OU=<OU>,DC=<DOMAIN>,DC=<DOMAIN>" -name * -scope subtree -limit 0 | dsget user -samid -pwdneverexpires | findstr /V no
Get-WmiObject -Class win32_group -Filter "Domain='<DOMAIN>'" | Select Caption,Name	
```
### Tools
#### Python anonymous bind
```
python3
>>> from ldap3 import *
>>> s = Server('10.129.1.207',get_info = ALL)
>>> c =  Connection(s, '', '')
>>> c.bind()
True
>>> s.info
```
#### LDAPSearch
```
#Anonymous bind
ldapsearch -H ldap://<IP> -x -b "dc=<DOMAIN>,dc=<DOMAIN>"

#Users with unconstrained deleg
ldapsearch -x -H ldap://<IP> -b "DC=<DOMAIN>,DC=<DOMAIN>" "(&(objectClass=group)(CN=Protected Users))" member
```
#### Windapsearch
```
python3 windapsearch.py --dc-ip <IP> -u "" --functionality						#confirm anonymous bind and get domain function level
python3 windapsearch.py --dc-ip <IP> -u "" -U									#pull list of all domain users
python3 windapsearch.py --dc-ip <IP> -u "" -C									#get information about all domain computers
python3 windapsearch.py --dc-ip 10.129.1.207 -u inlanefreight\\james.cross --da
python3 windapsearch.py --dc-ip 10.129.1.207 -d inlanefreight.local -u inlanefreight\\james.cross --unconstrained-users																	#users/computers with unconstrained deleg
python3 windapsearch.py --dc-ip 10.129.42.188 -u inlanefreight\\james.cross --custom "(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=128))"							#using LDAP filter
```
#### LDAPSearch-AD
```
python3 ldapsearch-ad.py -l <IP> -t info
python3 ldapsearch-ad.py -l 10.129.1.207 -d inlanefreight -u james.cross -p Summer2020 -t pass-pols																						#password policy
python3 ldapsearch-ad.py -l 10.129.1.207 -d inlanefreight -u james.cross -p Summer2020 -t kerberoast | grep servicePrincipalName:														#kerberoastable users
python3 ldapsearch-ad.py -l 10.129.1.207 -d inlanefreight -u james.cross -p Summer2020 -t asreproast																					#ASREProastable users
```