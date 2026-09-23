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

```