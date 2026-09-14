## 👤 Author

Emmanuel Adie Ushie  
Linux Essentials: WADF-2026-M01  
International Cybersecurity and Digital Forensics Academy

## Week 03: Bash Scripting, Hardware Awareness and Data Storage

This folder contains the hands-on laboratory work completed during Week 03 of
the Linux Essentials module at the International Cybersecurity and Digital
Forensics Academy (ICDFA).  
The week focused on writing reusable Bash scripts, validating user input,
automating repetitive tasks with loops, collecting a read-only hardware and
operating-system inventory, mapping Linux storage architecture, identifying
common data locations, and investigating disk usage safely.  
The documentation is structured for both academic submission and
professional GitHub portfolio presentation, with reproducible commands,
technical explanations, findings, and dedicated screenshot evidence.

## 📘 Week 03 Learning Areas

### Lab 5: Bash Scripting & Hardware Inventory

This lab covers Bash automation, defensive input handling, and read-only
system inventory collection.  
Topics covered:
- Creating reusable Bash scripts with shebangs and comments
- Using variables and command substitution
- Granting and verifying script execute permissions
- Capturing script output with `tee`
- Accepting user input with `read`
- Validating input with conditional statements
- Returning machine-readable exit status codes
- Automating file creation with `for` loops
- Testing successful and controlled failure paths
- Collecting CPU, memory, storage, network, and virtualisation information  
📑 Lab Report:
Lab05\_BashScripting\_HardwareInventory\_Report.md

### Lab 6: Storage Architecture & Disk Usage

This lab focuses on understanding Linux storage, common data locations, and
safe disk-capacity investigation.  
Topics covered:
- Mapping block devices, partitions, filesystems, and mount points
- Inspecting filesystem UUIDs and startup mount configuration
- Identifying persistent, temporary, and virtual data locations
- Distinguishing `/proc` and `/sys` from disk-backed directories
- Reviewing filesystem capacity with `df`
- Measuring directory usage with `du`
- Identifying the largest files with `find -printf`
- Inspecting hidden files in the home directory
- Developing evidence-based storage recommendations  
📑 Lab Report:
Lab06\_StorageArchitecture\_DiskUsage\_Report.md

## 🧪 Tools and Environment
- Kali Linux — Linux laboratory operating system
- VirtualBox — virtualisation platform used to run the Linux VM
- Bash Shell — scripting and command-line environment
- nano — terminal-based script editor
- chmod — Linux permission-management utility
- tee — simultaneous terminal display and output capture utility
- lscpu, free, lsblk, df, ip, and systemd-detect-virt — system inventory tools
- findmnt, blkid, and /etc/fstab — storage and mount inspection resources
- du, find, sort, and ls — disk-usage and file-discovery utilities
- GitHub — documentation and academic submission platform

## 📁 Evidence Structure

Week03\_LinuxEssentials/
├── README.md
├── Lab05\_BashScripting\_HardwareInventory\_Report.md
├── Lab06\_StorageArchitecture\_DiskUsage\_Report.md
└── Screenshots/
├── Fig01\_CLI\_ScriptCreation\_NanoSystemGreeting\_NlBa.png
├── Fig02\_CLI\_ScriptPermissionsAndExecution\_ChmodUx\_TeeOutput.png
├── Fig03\_CLI\_InputValidationScript\_CreateNotes\_SuccessAndFailure.png
├── Fig04\_CLI\_HardwareInventory\_Lscpu\_FreeH\_LsblkO.png
├── Fig05\_CLI\_DiskAndNetworkInventory\_DfHT\_IpBrLink.png
├── Fig06\_CLI\_VirtualizationDetection\_SystemdDetectVirt\_InventoryFile.png
├── Fig07\_CLI\_StorageMap\_LsblkF\_Findmnt.png
├── Fig08\_CLI\_FilesystemTableAndUuid\_CatFstab\_Blkid.png
├── Fig09\_CLI\_DataLocationDiscovery\_LsLd\_FindVarLog.png
├── Fig10\_CLI\_VirtualFilesystemVerification\_ProcUptime\_Stat.png
├── Fig11\_CLI\_DiskCapacityAndFolderSizes\_DfH\_DuSh\_DuMaxDepth.png
└── Fig12\_CLI\_LargestFilesAndHiddenFiles\_FindPrintf\_LsLa.png

## 📸 Screenshot Evidence

Screenshots are placed in the Screenshots/ directory and embedded in the
relevant lab reports immediately after the commands or activities they verify.

| Figure | Evidence | Report Section |
|---|---|---|
| Fig 1 | System greeting script creation | Lab 5 — 5A |
| Fig 2 | Script permissions, execution, and output capture | Lab 5 — 5A |
| Fig 3 | Input validation, success path, and failure path | Lab 5 — 5B |
| Fig 4 | CPU, memory, and block-device inventory | Lab 5 — 5C |
| Fig 5 | Disk and network inventory | Lab 5 — 5C |
| Fig 6 | Virtualisation detection and inventory file | Lab 5 — 5C |
| Fig 7 | Storage map and mounted filesystem tree | Lab 6 — 6A |
| Fig 8 | Filesystem table and UUID inspection | Lab 6 — 6A |
| Fig 9 | Common Linux data-location discovery | Lab 6 — 6B |
| Fig 10 | Virtual filesystem verification | Lab 6 — 6B |
| Fig 11 | Disk capacity and directory sizes | Lab 6 — 6C |
| Fig 12 | Largest files and hidden-file inspection | Lab 6 — 6C |

Evidence standard: Screenshots should clearly show the relevant command and
its output. Avoid unnecessary desktop content, unrelated applications, or
cropping that removes important context.

## 🎯 Learning Outcomes

By completing Week 03, I developed practical ability to:
- Write executable Bash scripts with clear comments and reusable variables.
- Use command substitution to collect live system information.
- Capture command and script output as evidence with `tee` and redirection.
- Validate user input before using it in file paths or automated operations.
- Use conditions, loops, and exit codes in defensive scripts.
- Collect a read-only hardware and operating-system baseline.
- Map the relationship between disks, partitions, filesystems, and mount points.
- Interpret UUIDs and review persistent mount configuration.
- Identify common Linux data locations and their persistence characteristics.
- Distinguish disk-backed storage from temporary and virtual filesystems.
- Measure filesystem and directory usage without deleting data.
- Identify large files and prepare evidence-based storage recommendations.  
These skills support automation, system baselining, incident response,
storage analysis, and live digital-forensics collection.

## 🔐 Evidence and Safety

All exercises were performed in a controlled virtual laboratory environment.
- Hardware, network, storage, and filesystem investigation used read-only commands.
- No disks were partitioned, formatted, mounted, or unmounted.
- Script-generated files were restricted to the designated Week 03 workspace.
- User input was validated before being incorporated into directory paths.
- Both successful and controlled failure paths were tested and documented.
- No files were deleted during the disk-usage investigation.
- Screenshots and output files were retained as reproducible evidence.

## 📌 Disclaimer

This repository is intended for educational and academic purposes only. All
practical activities were performed in controlled laboratory environments
using virtual machines and test data. No unauthorised system or third-party
infrastructure was targeted.

## 📚 Reference

ICDFA. (2026). WADF-2026-M01 Student Laboratory Workbook. Week 03: Bash
Scripting, Hardware Awareness and Data Storage. International Cybersecurity
and Digital Forensics Academy.

