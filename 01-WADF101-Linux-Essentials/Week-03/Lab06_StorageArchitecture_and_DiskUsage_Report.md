# Lab 6: Storage Architecture and Disk Usage

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | Linux Essentials: WADF-2026-M01 |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 03 |
| Lab | Lab 6: Storage Architecture and Disk Usage |
| Date of Submission | 09/07/2026 |

---

## Executive Summary

This report documents the practical activities completed in Lab 6 of the Week 03 Linux Essentials module. The lab focused on mapping Linux storage architecture, identifying common data locations, distinguishing persistent storage from temporary and virtual filesystems, and investigating disk usage without deleting or altering data.

Read-only commands were used to identify block devices, partitions, filesystem types, UUIDs, mount points, and startup mount configuration. Key directories such as `/home`, `/etc`, `/var/log`, `/tmp`, `/run`, `/proc`, and `/sys` were examined and classified. Filesystem capacity and directory usage were then assessed, and the largest files in the laboratory workspace were identified. These activities support storage troubleshooting and provide important context for digital-forensics acquisition and analysis.

---

## Objectives

- Map the relationship between block devices, partitions, filesystems, and mount points.
- Review filesystem UUIDs and startup mount configuration without making changes.
- Identify where Linux stores user files, configuration, logs, temporary data, installed software, runtime state, and kernel information.
- Distinguish persistent directories from temporary and virtual filesystems.
- Measure filesystem capacity and directory usage with read-only commands.
- Identify the largest files in the laboratory workspace.
- Inspect hidden files in the home directory.
- Develop evidence-based recommendations for storage management without deleting evidence.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Linux environment used for storage analysis.
- **`lsblk`:** Displayed block devices, partitions, filesystem types, UUIDs, and mount points.
- **`findmnt`:** Displayed the active mount hierarchy.
- **`df`:** Reported mounted filesystem capacity and usage.
- **`blkid`:** Displayed filesystem identifiers and types.
- **`/etc/fstab`:** Provided the system's configured startup mount information.
- **`ls`, `find`, `cat`, and `stat`:** Inspected directories, files, virtual data, and metadata.
- **`du`:** Measured directory and subdirectory sizes.
- **`sort` and `head`:** Ranked usage results and limited output for review.

---

## Workspace and Evidence Storage

```text
~/wadf-labs/week3/
~/wadf-labs/week3/output/
/media/sf_ICDFA/Week03/Screenshots/
/media/sf_ICDFA/Week03/Notes/
```

---

## Methodology

### 6A. Mapping Devices, Partitions, and Filesystems

The storage configuration was inspected with read-only commands. No partitioning, formatting, mounting, or unmounting operations were performed.

#### Display block devices and filesystems

```bash
lsblk -f
```

The `-f` option included filesystem type, label, UUID, available capacity, utilisation, and mount-point information where supported.

#### Display the active mount hierarchy

```bash
findmnt
```

`findmnt` presented mounted filesystems as a tree, showing how storage and virtual filesystems connect to the Linux directory hierarchy.

#### Review mounted filesystem capacity

```bash
df -hT
```

The command displayed filesystem type, total capacity, used space, available space, utilisation percentage, and mount point in human-readable units.

#### Review startup mount configuration

```bash
cat /etc/fstab
```

The filesystem table was read without editing. Its entries define filesystems that may be mounted automatically and the options used for them.

#### Display filesystem identifiers

```bash
blkid 2>/dev/null | head -n 20
```

`blkid` displayed available filesystem UUIDs, labels, and types. Error output was suppressed, and the displayed result was limited to the first 20 lines.

> **Storage map:** Replace the following example with the exact values shown by your final `lsblk -f` evidence: device `sda`, partition `sda1`, filesystem `ext4`, mount point `/`.

**Screenshot Filename:** `Fig07_CLI_StorageMap_LsblkF_Findmnt.png`

<img width="1366" height="662" alt="Fig07_CLI_StorageMap_LsblkF_Findmnt png" src="https://github.com/user-attachments/assets/3a4f5dd4-ed50-4301-928b-1136aa539188" />

**Screenshot Filename:** `Fig08_CLI_FilesystemTableAndUuid_CatFstab_Blkid.png`

<img width="1366" height="662" alt="Fig08_CLI_FilesystemTableAndUuid_CatFstab_Blkid png" src="https://github.com/user-attachments/assets/1629ee05-8fde-4ecb-b1ab-11b446d9e9d7" />

---

### 6B. Locating Common Linux Data Categories

Important Linux directories were examined and classified according to their purpose and persistence characteristics.

```bash
ls -ld /home /etc /var /tmp /usr /opt /run /proc /sys
find /var/log -maxdepth 1 -type f 2>/dev/null | head
cat /proc/uptime
stat /tmp /run /proc
```

`ls -ld` displayed directory-level ownership and permissions. The `find` command confirmed the presence of log files in `/var/log`. `/proc/uptime` demonstrated dynamically generated kernel information, while `stat` displayed metadata for selected temporary and virtual locations.

#### Data-location map

| Directory | Data Category | Persistence Characteristic |
|---|---|---|
| `/home` | Personal user files | Normally persistent across reboots |
| `/etc` | System configuration | Persistent |
| `/var/log` | System log files | Persistent, subject to rotation and retention policies |
| `/tmp` | Temporary files | Temporary; cleanup behaviour depends on system policy |
| `/usr` | Installed software and shared resources | Persistent |
| `/opt` | Optional or third-party packages | Persistent |
| `/run` | Runtime state | Volatile and normally recreated at boot |
| `/proc` | Process and kernel information | Virtual, generated by the kernel |
| `/sys` | Device, driver, and kernel object information | Virtual, generated by the kernel |

`/proc` and `/sys` are virtual filesystems, so their live contents are not ordinary disk-backed files. Relevant volatile information must therefore be captured from a live system when authorised and required. A forensic disk image may still contain configuration or logs related to system activity, but it does not preserve the live runtime contents presented through these virtual filesystems.

#### Save the completed data-location map

```bash
cat > ~/wadf-labs/week3/output/data_location_map.txt
```

The completed worksheet was entered and saved by pressing `Ctrl+D` at the start of a new line.

**Screenshot Filename:** `Fig09_CLI_DataLocationDiscovery_LsLd_FindVarLog.png`

<img width="1366" height="662" alt="Fig09_CLI_DataLocationDiscovery_LsLd_FindVarLog png" src="https://github.com/user-attachments/assets/082a3e05-e7f2-40eb-9d2b-b427191dc721" />

**Screenshot Filename:** `Fig10_CLI_VirtualFilesystemVerification_ProcUptime_Stat.png`

<img width="1366" height="662" alt="Fig10_CLI_VirtualFilesystemVerification_ProcUptime_Stat png" src="https://github.com/user-attachments/assets/784df10e-cef3-4ad8-a95e-44c04de520a5" />


### 6C. Investigating Disk Usage and Capacity

Disk usage was investigated without deleting or modifying files.

#### Review root and home filesystem capacity

```bash
df -h / ~
```

This command reported capacity and available space for the filesystems containing `/` and the current user's home directory. If both paths reside on the same filesystem, the displayed values may be identical.

#### Measure the complete laboratory workspace

```bash
du -sh ~/wadf-labs
```

`du -sh` calculated one human-readable total for the laboratory directory.

#### Compare immediate subdirectory sizes

```bash
du -h --max-depth=1 ~/wadf-labs | sort -h
```

The command measured the workspace and its immediate subdirectories, then sorted the results from smallest to largest.

#### Identify the five largest files

```bash
find ~/wadf-labs -type f -printf '%s %p\n' | sort -nr | head -n 5
```

`find -printf` printed each regular file's size in bytes and full path. Numeric reverse sorting placed the largest files first, and `head` limited the result to five entries.

#### Inspect hidden files in the home directory

```bash
ls -la ~ | head -n 30
```

The command displayed the first 30 entries in the home directory, including dotfiles. This provided a concise inspection but not a complete size ranking of hidden data.

**Screenshot Filename:** `Fig11_CLI_DiskCapacityAndFolderSizes_DfH_DuSh_DuMaxDepth.png`

<img width="1366" height="662" alt="Fig11_CLI_DiskCapacityAndFolderSizes_DfH_DuSh_DuMaxDepth png" src="https://github.com/user-attachments/assets/1fd26abc-cc37-471b-9beb-5372a0e950cd" />


**Screenshot Filename:** `Fig12_CLI_LargestFilesAndHiddenFiles_FindPrintf_LsLa.png`

<img width="1366" height="662" alt="Fig12_CLI_LargestFilesAndHiddenFiles_FindPrintf_LsLa png" src="https://github.com/user-attachments/assets/86129cde-c123-40a8-af45-60ff22fe5ce5" />

---

## Results and Findings

The storage-mapping exercise connected Linux pathnames to their underlying storage architecture. `lsblk -f`, `findmnt`, `df -hT`, `/etc/fstab`, and `blkid` provided complementary views of devices, filesystem formats, identifiers, active mounts, and configured mounts. The final device and partition values should be taken directly from the captured VM evidence rather than assumed from an example.

The data-location investigation distinguished persistent, temporary, volatile, and virtual locations. `/home`, `/etc`, `/usr`, and `/opt` normally store persistent data. `/run` is generally volatile, while `/proc` and `/sys` expose live kernel-generated information. `/tmp` is intended for temporary files, but its exact cleanup timing depends on operating-system policy, so data should not be assumed to disappear specifically at every reboot.

The capacity investigation identified how storage was distributed across the lab workspace. `du` measured directories, while `find -printf` supported a byte-based ranking of individual files. No cleanup was performed, preserving the evidence state during analysis.

---

## Challenges and Solutions

| Challenge | Cause | Resolution |
|---|---|---|
| Storage values in the draft were presented as an example. | The exact device, partition, filesystem, and mount point depend on the VM's actual configuration. | The report retains a clear placeholder instruction to replace the example with values from the final `lsblk -f` evidence. |
| Temporary and virtual directories required careful interpretation. | `/tmp`, `/run`, `/proc`, and `/sys` do not share the same persistence behaviour. | The directories were classified separately, and `/tmp` cleanup was described as policy-dependent rather than guaranteed at reboot. |

---

## Screenshot Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 7 | `Fig07_CLI_StorageMap_LsblkF_Findmnt.png` | 6A |
| Fig 8 | `Fig08_CLI_FilesystemTableAndUuid_CatFstab_Blkid.png` | 6A |
| Fig 9 | `Fig09_CLI_DataLocationDiscovery_LsLd_FindVarLog.png` | 6B |
| Fig 10 | `Fig10_CLI_VirtualFilesystemVerification_ProcUptime_Stat.png` | 6B |
| Fig 11 | `Fig11_CLI_DiskCapacityAndFolderSizes_DfH_DuSh_DuMaxDepth.png` | 6C |
| Fig 12 | `Fig12_CLI_LargestFilesAndHiddenFiles_FindPrintf_LsLa.png` | 6C |


---

## Recommendations

- Replace the example storage map with exact values from the VM evidence before submission.
- Use UUIDs where stable filesystem identification is required, while recognising that UUIDs should be verified against the current system.
- Treat `/proc`, `/sys`, and other volatile sources as live-response data and capture them promptly when authorised.
- Do not assume that `/tmp` is cleared at every reboot; verify the system's cleanup policy.
- Use `df` for filesystem capacity and `du` for directory-level usage because they measure different aspects of storage.
- Review the largest files before proposing archiving or deletion.
- Preserve original evidence and obtain authorisation before performing cleanup.
- Record commands, timestamps, and output paths so storage investigations remain reproducible.

---

## Conclusion

Lab 6 provided a practical understanding of the relationship between Linux devices, partitions, filesystems, and mount points. It also established where major categories of Linux data are stored and clarified the difference between persistent disk-backed data and live virtual filesystem content.

The disk-usage investigation demonstrated how to assess capacity, compare directory sizes, and identify large files without changing the system. These skills are directly relevant to storage administration, incident response, and digital forensics, where analysts must understand both the logical filesystem hierarchy and the physical or virtual storage beneath it.

---

## Safety and Evidence Handling

All commands used in this lab were read-only inspection commands. No device was partitioned, formatted, mounted, unmounted, or modified, and no file was deleted during the capacity investigation. Exact system values should be drawn from retained command output and screenshots to avoid unsupported assumptions.

---

## Reference

ICDFA. (2026). *WADF-2026-M01 Student Laboratory Workbook: Week 03, Bash Scripting, Hardware Awareness and Data Storage.* International Cybersecurity and Digital Forensics Academy.

