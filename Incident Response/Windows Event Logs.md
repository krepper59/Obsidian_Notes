Review with Windows Event Viewer (run as admin)
### Event Logs
```
Information: general application usage (start/stop/etc)
Error: specific error events with detailed information

Components:
	1. Log Name: The name of the event log (e.g., Application, System, Security, etc.).
	2. Source: The software that logged the event.
	3. Event ID: A unique identifier for the event.
	4. Task Category: This often contains a value or name that can help us understand the purpose or use of the event.
	5. Level: The severity of the event (Information, Warning, Error, Critical, and Verbose).
	6. Keywords: Keywords are flags that allow us to categorize events in ways beyond the other classification options. These are generally broad categories, such as "Audit Success" or "Audit Failure" in the Security log.
	7. User: The user account that was logged on when the event occurred.
	8. OpCode: This field can identify the specific operation that the event reports.
	9. Logged: The date and time when the event was logged.
	10. Computer: The name of the computer where the event occurred.
	11. XML Data: All the above information is also included in an XML format along with additional event data.
```
#### Custom XML Queries
```
Filter Current Log -> XML -> Edit Query Manually
```
### Useful Logs
#### Windows System Logs
```
Event ID 1074 (System Shutdown/Restart)
Event ID 6005 (The Event log service was started)
Event ID 6006 (The Event log service was stopped)
Event ID 6013 (Windows uptime)
Event ID 7040 (Service status change)
```
#### Windows Security Logs
```
Event ID 1102 (The audit log was cleared)
Event ID 1116 (Antivirus malware detection)
Event ID 1118 (Antivirus remediation activity has started)
Event ID 1119 (Antivirus remediation activity has succeeded)
Event ID 1120 (Antivirus remediation activity has failed)
Event ID 4624 (Successful Logon)
Event ID 4625 (Failed Logon)
Event ID 4648 (A logon was attempted using explicit credentials)
Event ID 4656 (A handle to an object was requested)
Event ID 4672 (Special Privileges Assigned to a New Logon)
Event ID 4698 (A scheduled task was created)
Event ID 4700 & Event ID 4701 (A scheduled task was enabled/disabled)
Event ID 4702 (A scheduled task was updated)
Event ID 4719 (System audit policy was changed)
Event ID 4738 (A user account was changed)
Event ID 4771 (Kerberos pre-authentication failed)
Event ID 4776 (The domain controller attempted to validate the credentials for an account)
Event ID 5001 (Antivirus real-time protection configuration has changed)
Event ID 5140 (A network share object was accessed)
Event ID 5142 (A network share object was added)
Event ID 5145 (A network share object was checked to see whether client can be granted desired access)
Event ID 5157 (The Windows Filtering Platform has blocked a connection)
Event ID 7045 (A service was installed in the system)
```
### Sysmon
```
Components:
1. A Windows service for monitoring system activity.
2. A device driver that assists in capturing the system activity data.
3. An event log to display captured activity data.

Event Viewer:
Applications and Services -> Microsoft -> Windows -> Sysmon

Sysmon Event ID's: 
https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon

Configurations:
https://github.com/SwiftOnSecurity/sysmon-config
https://github.com/olafhartong/sysmon-modular

Install:
sysmon.exe -i -accepteula -h md5,sha256,imphash -l -n
sysmon.exe -c configuration.xml

Sysmon for Linux:
https://github.com/Sysinternals/SysmonForLinux
```
### Event Tracing
```
Components:
	1. Controllers - starts and stops trace sessions and enables or disables providers in a trace
	2. Providers - generates events and writes them to ETW sessions, 4 types
		1. MOF Providers: Managed Object Format, generate events based on MOF schema
		2. WPP Providers: Windows Software Trace Preprocessor, provide specialized macros within the apps source code to generate events
		3. Manifest-based Providers: use XML manifest files to define structure and characteristics of events
		4. TraceLogging Providers: Use TraceLoggingAPI with minimal overhead to generate events
	3. Consumers - subscribe to specific events for further processing and analysis
	4. Channels - containers for organizing and filtering events based on characteristics or importance
	5. ETL Files - events written to disk using trace log files to store events
	   
Interaction:
	Logman - tool for interacting with ETW
	logman.exe query -ets
	logman.exe query "EventLog-System" -ets
	logmam.exe query providers
	logman.exe query providers | findstr "Winlogon"
	logman.exe query providers Microsoft-Windows-Winlogon
	
	Performance monitor also has a tab for ETW sessions
```
###### Useful Providers
```
Microsoft-Windows-Kernel-Process: This ETW provider is instrumental in monitoring process-related activity within the Windows kernel. It can aid in detecting unusual process behaviors such as process injection, process hollowing, and other tactics commonly used by malware and advanced persistent threats (APTs).

Microsoft-Windows-Kernel-File: As the name suggests, this provider focuses on file-related operations. It can be employed for detection scenarios involving unauthorized file access, changes to critical system files, or suspicious file operations indicative of exfiltration or ransomware activity.

Microsoft-Windows-Kernel-Network: This ETW provider offers visibility into network-related activity at the kernel level. It's especially useful in detecting network-based attacks such as data exfiltration, unauthorized network connections, and potential signs of command and control (C2) communication.

Microsoft-Windows-SMBClient/SMBServer: These providers monitor Server Message Block (SMB) client and server activity, providing insights into file sharing and network communication. They can be used to detect unusual SMB traffic patterns, potentially indicating lateral movement or data exfiltration.

Microsoft-Windows-DotNETRuntime: This provider focuses on .NET runtime events, making it ideal for identifying anomalies in .NET application execution, potential exploitation of .NET vulnerabilities, or malicious .NET assembly loading.

OpenSSH: Monitoring the OpenSSH ETW provider can provide important insights into Secure Shell (SSH) connection attempts, successful and failed authentications, and potential brute force attacks.

Microsoft-Windows-VPN-Client: This provider enables tracking of Virtual Private Network (VPN) client events. It can be useful for identifying unauthorized or suspicious VPN connections.

Microsoft-Windows-PowerShell: This ETW provider tracks PowerShell execution and command activity, making it invaluable for detecting suspicious PowerShell usage, script block logging, and potential misuse or exploitation.

Microsoft-Windows-Kernel-Registry: This provider monitors registry operations, making it useful for detection scenarios related to changes in registry keys, often associated with persistence mechanisms, malware installation, or system configuration changes.

Microsoft-Windows-CodeIntegrity: This provider monitors code and driver integrity checks, which can be key in identifying attempts to load unsigned or malicious drivers or code.

Microsoft-Antimalware-Service: This ETW provider can be employed to detect potential issues with the antimalware service, including disabled services, configuration changes, or potential evasion techniques employed by malware.

WinRM: Monitoring the Windows Remote Management (WinRM) provider can reveal unauthorized or suspicious remote management activity, often indicative of lateral movement or remote command execution.

Microsoft-Windows-TerminalServices-LocalSessionManager: This provider tracks local Terminal Services sessions, making it useful for detecting unauthorized or suspicious remote desktop activity.

Microsoft-Windows-Security-Mitigations: This provider keeps tabs on the effectiveness and operations of security mitigations in place. It's essential for identifying potential bypass attempts of these security controls.

Microsoft-Windows-DNS-Client: This ETW provider gives visibility into DNS client activity, which is crucial for detecting DNS-based attacks, including DNS tunneling or unusual DNS requests that may indicate C2 communication.

Microsoft-Antimalware-Protection: This provider monitors the operations of antimalware protection mechanisms. It can be used to detect any issues with these mechanisms, such as disabled protection features, configuration changes, or signs of evasion techniques employed by malicious actors.
```
![[Pasted image 20260924080618.png]]
### Get-WinEvent
###### List Providers:
```
Get-WinEvent -ListLog * | Select-Object LogName, RecordCount, IsClassicLog, IsEnabled, LogMode, LogType | Format-Table -AutoSize
```
###### Get events from System logs:
```
Get-WinEvent -LogName 'System' -MaxEvents 50 | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize
```
###### Get events from Microsoft-Windows-WinRM/Operational:
```
Get-WinEvent -LogName 'Microsoft-Windows-WinRM/Operational' -MaxEvents 30 | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize
```
###### Oldest events:
```
Get-WinEvent -LogName 'Microsoft-Windows-WinRM/Operational' -Oldest -MaxEvents 30 | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize
```
###### Get events from an evtx file:
```
Get-WinEvent -Path 'C:\Tools\chainsaw\EVTX-ATTACK-SAMPLES\Execution\exec_sysmon_1_lolbin_pcalua.evtx' -MaxEvents 5 | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize
```
###### Filter events with -FilterHashTable:
```
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=1,3} | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize
```
###### From exported evtx:
```
Get-WinEvent -FilterHashtable @{Path='C:\Tools\chainsaw\EVTX-ATTACK-SAMPLES\Execution\sysmon_mshta_sharpshooter_stageless_meterpreter.evtx'; ID=1,3} | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize
```
###### Logs by date:
```
$startDate = (Get-Date -Year 2023 -Month 5 -Day 28).Date
$endDate   = (Get-Date -Year 2023 -Month 6 -Day 3).Date
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=1,3; StartTime=$startDate; EndTime=$endDate} | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize
```
###### Filter events with -FilterHashTable & XML:
```
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=3} |
`ForEach-Object {
$xml = [xml]$_.ToXml()
$eventData = $xml.Event.EventData.Data
New-Object PSObject -Property @{
    SourceIP = $eventData | Where-Object {$_.Name -eq "SourceIp"} | Select-Object -ExpandProperty '#text'
    DestinationIP = $eventData | Where-Object {$_.Name -eq "DestinationIp"} | Select-Object -ExpandProperty '#text'
    ProcessGuid = $eventData | Where-Object {$_.Name -eq "ProcessGuid"} | Select-Object -ExpandProperty '#text'
    ProcessId = $eventData | Where-Object {$_.Name -eq "ProcessId"} | Select-Object -ExpandProperty '#text'
}
}  | Where-Object {$_.DestinationIP -eq "52.113.194.132"}
```

```
$Query = @"
	<QueryList>
		<Query Id="0">
			<Select Path="Microsoft-Windows-Sysmon/Operational">*[System[(EventID=7)]] and *[EventData[Data='mscoree.dll']] or *[EventData[Data='clr.dll']]
			</Select>
		</Query>
	</QueryList>
"@

$Query | ForEach-Object {Write-Host $_.Message `n}
```
###### Filter events with FilterXPath:
```
Get-WinEvent -LogName 'Microsoft-Windows-Sysmon/Operational' -FilterXPath "*[EventData[Data[@Name='Image']='C:\Windows\System32\reg.exe']] and *[EventData[Data[@Name='CommandLine']='`"C:\Windows\system32\reg.exe`" ADD HKCU\Software\Sysinternals /v EulaAccepted /t REG_DWORD /d 1 /f']]" | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message | Format-Table -AutoSize
```

```
Get-WinEvent -LogName 'Microsoft-Windows-Sysmon/Operational' -FilterXPath "*[System[EventID=3] and EventData[Data[@Name='DestinationIp']='52.113.194.132']]"
```
###### Filter events based on property values:
```
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=1} -MaxEvents 1 | Select-Object -Property *
```

```
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=1} | Where-Object {$_.Properties[21].Value -like "*-enc*"} | Format-List
```
###### Challenge: Find when share \*\PRINT was added:
```
Get-WinEvent -Path "C:\Tools\chainsaw\EVTX-ATTACK-SAMPLES\Lateral Movement\*.evtx" -FilterXPath "*[System[EventID=5142]]"
```
###### Challenge: determine the process responsible for executing a DLL hijacking attack
```
Get-WinEvent -Path "C:\Logs\DLLHijack\*.evtx" -FilterXPath "*[System[EventID=7]]"
```

```
Get-WinEvent -Path "C:\Logs\DLLHijack\*.evtx" -FilterXPath "*[System[EventID=7]]" | 
ForEach-Object {
	$Xml = [xml]$_.ToXml()
	$EventData = $Xml.Event.EventData.Data 
	
	[PSCustomObject]@{
		TimeCreated = $_.TimeCreated
		Process = ($EventData | Where-Object { $_.Name -eq 'Image'}).'#text'
		DLLLoaded = ($EventData | Where-Object { $_.Name -eq 'ImageLoaded'}).'#text'
		Signed = ($EventData | Where-Object { $_.Name -eq 'Signed'}).'#text'
		Signature = ($EventData | Where-Object { $_.Name -eq 'Signature'}).'#text'
	} 
} | Out-GridView
```
###### Challenge: determine the process that executed unmanaged PowerShell code
	Look for System.Management.Automation.dll, clr.dll, clrjit.dll, mscoree.dll
```
Get-WinEvent -Path  "C:\Logs\PowershellExec\*.evtx" -FilterXPath "*[System[EventID=7]]" | 
ForEach-Object {
	$Xml = [xml]$_.ToXml()
	$EventData = $Xml.Event.EventData.Data 
	
	[PSCustomObject]@{
		TimeCreated = $_.TimeCreated
		Process = ($EventData | Where-Object { $_.Name -eq 'Image'}).'#text'
		DLLLoaded = ($EventData | Where-Object { $_.Name -eq 'ImageLoaded'}).'#text'
		Signed = ($EventData | Where-Object { $_.Name -eq 'Signed'}).'#text'
	} 
} | Out-GridView
```
Challenge: determine the process that injected into the process that executed unmanaged PowerShell code
```
Get-WinEvent -Path  "C:\Logs\PowershellExec\*.evtx" -FilterXPath "*[System[EventID=7]]" | 
ForEach-Object {
	$Xml = [xml]$_.ToXml()
	$EventData = $Xml.Event.EventData.Data 
	
	[PSCustomObject]@{
		TimeCreated = $_.TimeCreated
		Process = ($EventData | Where-Object { $_.Name -eq 'Image'}).'#text'
		DLLLoaded = ($EventData | Where-Object { $_.Name -eq 'ImageLoaded'}).'#text'
		Signed = ($EventData | Where-Object { $_.Name -eq 'Signed'}).'#text'
	} 
} | Out-GridView
```