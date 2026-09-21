### LDAP Filters
```
#Find all workstations
(objectCategory=computer)

#Find all domain controllers
(&(objectCategory=Computer)(userAccountControl:1.2.840.113556.1.4.803:=8192))

#Find all users
(&(objectCategory=person)(objectClass=user))

#Search all groups
(objectClass=group)

#Find disabled accounts
(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))

#Find all groups with <USER>
(member:1.2.840.113556.1.4.1941:=CN=<USER>,OU=<OU>,OU=<OU>,OU=<OU>,DC=<DOMAIN>,DC=<DOMAIN>)

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
