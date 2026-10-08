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
```
Prefetch Files - file paths, execution counts, and timestamps
Shimcache - file paths, execution timestamps, execution flags (good for recently executed)
Amcache - database of installed apps and exes with paths, sizes, digital signatures, and timestamps
UserAssist - registry key with info about programs executed by user like names and execution count
RunMRU Lists - store info about recently executed programs, like Run and RunOnce keys
Jump Lists - recently accessed files, folders, and tasks associated with specific applications
Shortcut (LNK) Files - file paths, timestamps, and user interactions with the target exe
Recent Items - list of recently opened files
Windows Event Logs - events related to program execution, application crashes, etc.
```
### Windows Persistence Artifacts
### Web Browser Forensics
### SRUM
