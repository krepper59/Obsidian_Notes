### Forensic Imaging
```
Creating an exact bit-by-bit copy of digital storage media (hard drive, SSD, USB)
Preserves state, ensures integrity, and maintains admissibility

Tools:
	FTKImager - creates perfect copies (images) of computer disks for viewing without altering
	AFF4 Imager - open-source tool for creating and dupicating images, able to extract files
	DD & DCFLDD - linux utilities for imaging
	Virtualization Tools - snapshots can be used just like an image
```
###### Example 1: FTK Imager
```
Select File -> Create Disk Image
Next, select the media source. Typically, it's either Physical Drive or Logical Drive
Choose the drive from which you wish to create an image.
Specify the destination for the image.
Select the desired image type.
Input evidence details.
Choose the destination folder and filename for the image & adjust settings for image compression.
Once all settings are confirmed, click Start.
You'll observe the progress of the imaging.
If you opted to verify the image, you'll also see the verification progress.
After the image has been verified, you'll receive an imaging summary.
```
### Volatile Host-Based Evidence

| Tool                   | Description                                                                                                                                                                                                                                                                                                                                         |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| WinPmem                | WinPmem has been the default open source memory acquisition driver for windows for a long time. It used to live in the Rekall project, but has recently been separated into its own repository.                                                                                                                                                     |
| DumpIt                 | A simplistic utility that generates a physical memory dump of Windows and Linux machines. On Windows, it concatenates 32-bit and 64-bit system physical memory into a single output file, making it extremely easy to use.                                                                                                                          |
| MemDump                | MemDump is a free, straightforward command-line utility that enables us to capture the contents of a system's RAM. It’s quite beneficial in forensics investigations or when analyzing a system for malicious activity. Its simplicity and ease of use make it a popular choice for memory acquisition.                                             |
| Belkasoft RAM Capturer | This is another powerful tool we can use for memory acquisition, provided free of charge by Belkasoft. It can capture the RAM of a running Windows computer, even if there's active anti-debugging or anti-dumping protection. This makes it a highly effective tool for extracting as much data as possible during a live forensics investigation. |
| Magnet RAM Capture     | Developed by Magnet Forensics, this tool provides a free and simple way to capture the volatile memory of a system.                                                                                                                                                                                                                                 |
| LiME                   | LiME is a Loadable Kernel Module (LKM) which allows the acquisition of volatile memory. LiME is unique in that it's designed to be transparent to the target system, evading many common anti-forensic measures.                                                                                                                                    |
###### Example 2: Acquiring Memory with WinPmem
```
To generate a memory dump, simply execute the command below with Administrator privileges.
winpmem_mini_x64_rc2.exe memdump.raw
```
###### Example 3: Acquiring VM Memory
```
Open the running VM's options
Suspend the running VM
Locate the .vmem file inside the VM's directory.
```
### Rapid Triage
###### KAPE
```
KAPE operates based on the principles of Targets and Modules. These elements guide the tool in processing data and extracting forensic artifacts. When we feed a source to KAPE, it duplicates specific forensic-related files to a designated output directory, all while maintaining the metadata of each file.

KAPE provides users with two modes: CLI (kape.exe) and GUI (gkape.exe).
Targets refer to the specific artifacts we aim to extract from an image or system. These are then duplicated to the output directory.

KAPE's target files have a .tkape extension and reside in the <path to kape>\KAPE\Targets directory.
KAPE also offers Compound Targets, which are essentially amalgamations of multiple targets. This feature accelerates the collection process by gathering multiple files defined across various targets in a single run. The Compound directory's KapeTriage file provides an overview of the contents of this compound target.
```
###### Velociraptor
```
Velociraptor is a potent tool for gathering host-based information using Velociraptor Query Language (VQL) queries. Beyond this, Velociraptor can execute Hunts to amass various artifacts. A frequently utilized artifact is the Windows.KapeFiles.Targets. While KAPE (Kroll Artifact Parser and Extractor) itself isn't open-source, its file collection logic, encoded in YAML, is accessible via the KapeFiles project.
```
###### Example: Velociraptor
```
Initiate a new Hunt
Choose Windows.KapeFiles.Targets as the artifacts for collection.
Specify the collection to use.
Click on Launch to start the hunt.
Once completed, download the results.
Extracting the archive will reveal files related to the collected artifacts and all gathered files.

For remote memory dump collection using Velociraptor:
Start a new Hunt, but this time, select the Windows.Memory.Acquisition artifact.
After the Hunt concludes, download the resulting archive. Within, you'll find a file named PhysicalMemory.raw, containing the memory dump.
```
