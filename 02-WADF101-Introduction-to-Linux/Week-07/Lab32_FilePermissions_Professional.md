# Lab 32: File Permissions

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 07 |
| Lab | Lab 32: File Permissions |
| Date of Submission | 06/08/2026 |


---

## Executive Summary

This report documents Lab 32 of Week 07. The lab examined Linux file types, owner-group-other permission sets, symbolic and numeric `chmod` notation, ownership changes, recursive operations, default permission masks, and special permission bits.

The activities demonstrated how Linux access controls govern reading, writing, execution, directory traversal, ownership, and deletion behaviour. Security implications were considered throughout, particularly for recursive mode changes and setuid permissions.

---

## Objectives

- Interpret long-listing permission strings.
- Calculate numeric permission values.
- Apply symbolic and numeric mode changes.
- Change owner and group assignments.
- Understand recursive permission changes and their risks.
- Set and verify a session umask.
- Demonstrate setuid and sticky-bit notation safely.

---

## Tools and Environment

- **`ls -l`:** Displayed file type, permissions, ownership, and metadata.
- **`chmod`:** Changed ordinary and special permission bits.
- **`chown` and `chgrp`:** Changed user and group ownership.
- **`umask`:** Displayed and changed the process file-creation mask.
- **`find`:** Supported safer type-specific recursive changes.

---

## Workspace and Evidence Storage

```text
~/archive_test/
~/archive_test/documents/
/media/sf_ICDFA/Week07/Screenshots/
```

---

## Methodology

### 32A. Read Linux Permission Strings

```bash
ls -l ~/archive_test/documents/
```

The first character identifies the entry type, such as `-` for a regular file or `d` for a directory. The next nine characters are divided into owner, group, and other permission triplets. Numeric values are `r=4`, `w=2`, and `x=1`.

**Screenshot Filename:** `Fig10_CLI_PermissionReadingAndSymbolicChmod_LsL_ChmodUx.png`

<img width="1366" height="662" alt="Fig10_CLI_PermissionReadingAndSymbolicChmod_LsL_ChmodUx png" src="https://github.com/user-attachments/assets/96a02f86-2885-4c1c-98db-af7d01274a72" />


---

### 32B. Apply Symbolic and Numeric Permissions

```bash
chmod u+x ~/archive_test/documents/file1.txt
ls -l ~/archive_test/documents/file1.txt
chmod 755 ~/archive_test/documents/file1.txt
chmod o-r ~/archive_test/documents/file1.txt
ls -l ~/archive_test/documents/file1.txt
```

`u+x` added owner execute permission. Mode `755` assigned `rwx` to the owner and `r-x` to group and others. `o-r` then removed read permission from others.

> A `.txt` file normally does not require execute permission. The setting was used only to demonstrate permission notation.

### 32C. Change Ownership

```bash
sudo chown root ~/archive_test/documents/file1.txt
sudo chgrp staff ~/archive_test/documents/file1.txt
sudo chown "$USER":staff ~/archive_test/documents/file1.txt
ls -l ~/archive_test/documents/file1.txt
```

The first command changed the owner to `root`, the second changed the group to `staff`, and the third restored the owner to the current account while retaining the selected group. The group must exist on the system.

**Screenshot Filename:** `Fig11_CLI_NumericChmodAndOwnershipChange_Chmod755_Chown_Chgrp.png`

<img width="1366" height="662" alt="Fig11_CLI_NumericChmodAndOwnershipChange_Chmod755_Chown_Chgrp png" src="https://github.com/user-attachments/assets/ea27564f-7df0-43cb-b905-fcc0da3dad3f" />


---

### 32D. Recursive Permissions and umask

The original exercise used:

```bash
chmod -R 755 ~/archive_test
```

This applies execute permission to every regular file as well as directories. A safer type-specific approach is:

```bash
find ~/archive_test -type d -exec chmod 755 {} +
find ~/archive_test -type f -exec chmod 644 {} +
```

Directories require execute permission for traversal, while ordinary data files generally do not.

```bash
umask 022
umask
```

A process umask of `022` typically produces mode `644` for newly created regular files and `755` for newly created directories, subject to the application's requested mode and other controls. Umask clears permission bits rather than performing ordinary arithmetic subtraction.

### 32E. Demonstrate Special Permission Bits

```bash
chmod u+s ~/archive_test/documents/file1.txt
ls -l ~/archive_test/documents/file1.txt
chmod u-s ~/archive_test/documents/file1.txt
chmod +t ~/archive_test
ls -ld ~/archive_test
```

The setuid bit was applied only to observe its notation and then removed. On Linux, setuid has meaningful effect on suitable executable binary files, not ordinary text files or shell scripts in the normal case. Leaving setuid on an unnecessary executable would create security risk.

The sticky bit on a writable directory restricts deletion or renaming of entries by users who do not own the entry or directory and lack elevated privilege.

**Screenshot Filename:** `Fig12_CLI_RecursivePermissionsUmaskAndSpecialBits_ChmodR_Umask_Setuid_Sticky.png`

<img width="1366" height="662" alt="Fig12_CLI_RecursivePermissionsUmaskAndSpecialBits_ChmodR_Umask_Setuid_Sticky png" src="https://github.com/user-attachments/assets/9e596f7c-4a05-4438-8f60-d65336571838" />


---

## Results and Findings

The lab connected symbolic permission notation with its numeric representation. It also showed that file permissions and directory permissions have different practical meanings. Execute permission on a directory controls traversal, while execute permission on a regular file controls whether the kernel may attempt to run it.

Recursive `755` is convenient but often over-permissive for regular data files. Type-specific `find` operations provide a safer alternative. The exercise also clarified that setuid should be treated as a high-risk privilege mechanism and should not remain enabled merely for demonstration.

---

## Challenges and Solutions

| Challenge | Cause | Resolution |
|---|---|---|
| A placeholder username could not be resolved by `chown`. | `username` was an example rather than an existing account. | `$USER` or an actual verified account name was used. |
| Recursive `755` made ordinary data files executable. | `chmod -R` applied one mode to both files and directories. | Type-specific `find` commands assigned `755` to directories and `644` to regular files. |

---

## Screenshot Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 10 | `Fig10_CLI_PermissionReadingAndSymbolicChmod_LsL_ChmodUx.png` | 32A–32B |
| Fig 11 | `Fig11_CLI_NumericChmodAndOwnershipChange_Chmod755_Chown_Chgrp.png` | 32B–32C |
| Fig 12 | `Fig12_CLI_RecursivePermissionsUmaskAndSpecialBits_ChmodR_Umask_Setuid_Sticky.png` | 32D–32E |

---

## Recommendations

- Verify each permission or ownership change immediately with `ls -l` or `stat`.
- Avoid recursive permission modes that make ordinary data files executable.
- Confirm that users and groups exist before using `chown` or `chgrp`.
- Treat setuid as a privileged security control and remove it after demonstrations.
- Set umask in the appropriate shell or configuration scope and verify newly created test entries.
- Apply least privilege rather than relying on broad modes such as `777`.

---

## Conclusion

Lab 32 established a practical model of Linux discretionary access control. Symbolic and numeric permissions, ownership, recursive operations, umask, setuid, and sticky-bit behaviour were examined and verified. These skills are essential for system hardening, access review, and the investigation of permission-based persistence or exposure.

---

## Safety and Evidence Handling

All ownership and mode changes were limited to controlled laboratory files. Special bits were demonstrated temporarily, and safer final permissions were documented.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 7 Practical Labs: Labs 29–32, Standard Text Streams, Processes, Archives and File Permissions.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
3. GNU Project. (n.d.). *GNU tar Manual.* https://www.gnu.org/software/tar/manual/
