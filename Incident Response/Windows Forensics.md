### NTFS
```
File Metadata - creation, access, modification times and attribute information
MFT Entries - file names, sizes, timestamps, storage locations
File Slack and Unallocated Space - remnants of deleted files or data fragments
File Signatures - file types and headers
USN Journal - Update Sequence Number - records changes made to files and directories
LNK Files - shortcut files with info about target file or program with timestamps and metadata
Prefetch Files - indicate which programs have been run and when
Registry Hives - configuration and system information
Shellbags - registry entries to store folder view settings, can identify folder access
Thumbnail Cache - store mini previews of images and documents, can reveal viewed files
Recycle Bin - files deleted from filesystem
Alternate Data Streams (ADS) - streams that can be used to hide data
Volume Shadow Copies - snapshots of the filesystem
Security Descriptors & ACL's - file and folder permissions
```
### Windows Event Logs
```
Stored at C:\Windows\System32\winevt\logs
```
### Execution Artifacts

| Artifact             | Location                                                             | Data                                                                           |
| -------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Prefetch             | C:\Windows\Prefetch                                                  | Metadata about executed applications (file paths, timestamps, execution count) |
| Shimcache            | HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache | Program execution details (file paths, timestamps, flags)                      |
| Amcache              | C:\Windows\AppCompat\Programs\Amcache.hve                            | Application details (file paths, sizes, digital signatures, timestamps)        |
| UserAssist           | HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist   | Executed program details (application names, execution counts, timestamps)     |
| RunMRU               | HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU       | Recently executed programs and their command lines                             |
| Jump Lists           | %AppData%\Microsoft\Windows\Recent        (User Specific)            | Recently accessed files, folders, and tasks associated with applications       |
| Shortcut (LNK) Files | Various locations (e.g., Desktop, Start Menu)                        | Target executable, file paths, timestamps, user interactions                   |
| Recent Items         | %AppData%\Microsoft\Windows\Recent        (User Specific)            | Recently accessed files                                                        |
| Windows Event Logs   | C:\Windows\System32\winevt\Logs                                      | Various event logs containing process creation, termination, and other events  |

### Windows Persistence Artifacts
###### Registry
```
Database that stores critical Windows OS settings

Run/RunOnce Keys:
	HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
	HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce
	HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
	HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
	HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\

WinLogon Keys:
	HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon
	HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell

Startup Keys:
	HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders
	HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders
	HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders
	HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User
```
###### Schtasks
```
C:\Windows\System32\Tasks - This file details the creator, the task's timing or trigger, and the path to the command or program set to run.
```
###### Services
```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services - Pivotal for maintaining processes on a system, enabling software components to operate in the background without user intervention
```
### Web Browser Forensics

| Artifact               | Description                                                                                                                                     |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Browsing History       | Records of websites visited, including URLs, titles, timestamps, and visit frequency.                                                           |
| Cookeis                | Small data files stored by websites on a user's device, containing information such as session details, preferences, and authentication tokens. |
| Cache                  | Cached copies of web pages, images, and other content visited by the user. Can reveal websites accessed even if the history is cleared.         |
| Bookmarks              | Saved links to frequently visited websites or pages of interest.                                                                                |
| Download History       | Records of downloaded files, including source URLs, filenames, and timestamps.                                                                  |
| Autofill Data          | Information automatically entered into forms, such as names, addresses, and passwords.                                                          |
| Search History         | Queries entered into search engines, along with search terms and timestamps.                                                                    |
| Session Data           | Information about active browsing sessions, tabs, and windows.                                                                                  |
| Typed URL's            | URLs entered directly into the address bar.                                                                                                     |
| Form Data              | Information entered into web forms, such as login credentials and search queries.                                                               |
| Passwords              | Saved or autofilled passwords for websites.                                                                                                     |
| Web Storgae            | Local storage data used by websites for various purposes.                                                                                       |
| Favicons               | Small icons associated with websites, which can reveal visited sites.                                                                           |
| Tab Recovery Data      | Information about open tabs and sessions that can be restored after a browser crash.                                                            |
| Extensions and Add-ons | Installed browser extensions and their configurations.                                                                                          |

### SRUM
```
System Resource Usage Monitor - tracks resource utilization and application usage patterns.
Location - C:\Windows\System32\sru\sru.db (SQLite database)
```

| Artifact                     | Description                                                                                                                                                                                                                                                                                                                                                                                                 |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Application Profiling        | SRUM can provide a comprehensive view of the applications and processes that have been executed on a Windows system. It records details such as executable names, file paths, timestamps, and resource usage metrics. This information is crucial for understanding the software landscape on a system, identifying potentially malicious or unauthorized applications, and reconstructing user activities. |
| Resource Consumption         | SRUM captures data on CPU time, network usage, and memory consumption for each application and process. This data is invaluable for investigating resource-intensive activities, identifying unusual patterns of resource consumption, and detecting potential performance issues caused by specific applications.                                                                                          |
| Timeline Reconstruction      | By analyzing SRUM data, digital forensics experts can create timelines of application and process execution, resource usage, and system activities. This timeline reconstruction is instrumental in understanding the sequence of events, identifying suspicious behaviors, and establishing a clear picture of user interactions and actions.                                                              |
| User and System Context      | SRUM data includes user identifiers, which helps in attributing activities to specific users. This can aid in user behavior analysis and determining whether certain actions were performed by legitimate users or potential threat actors.                                                                                                                                                                 |
| Malware Analysis & Detection | SRUM data can be used to identify unusual or unauthorized applications that may be indicative of malware or malicious activities. Sudden spikes in resource usage, abnormal application patterns, or unauthorized software installations can all be detected through SRUM analysis.                                                                                                                         |
| Incident Response            | During incident response, SRUM can provide rapid insights into recent application and process activities, enabling analysts to quickly identify potential threats and respond effectively.                                                                                                                                                                                                                  |
