# Lab 33: Filesystem Links

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 08 |
| Lab | Lab 33: Filesystem Links |
| Date of Submission | 14/08/2026 |


---

## Executive Summary

This lab examined the relationship between filenames, inodes, hard links, and symbolic links. A controlled file was created, its inode was inspected, and both link types were tested before and after the original pathname was removed.

---

## Objectives

- Explain the relationship between filenames and inodes.
- Create and verify hard links and symbolic links.
- Compare inode values and link counts.
- Demonstrate how deletion of a target affects each link type.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Week 08 laboratory environment.
- **Primary tools:** `ls -i`, `ls -il`, `ln`, `ln -s`, `cat`, `rm`.
- **VirtualBox:** Provided isolated hardware and a recoverable training platform.
- **GitHub:** Portfolio and academic documentation platform.

---

## Workspace and Evidence Storage

```text
/media/sf_ICDFA/Week08/
/media/sf_ICDFA/Week08/Screenshots/
/media/sf_ICDFA/Week08/Notes/
```

---

## Methodology

### 33A. Create and Inspect the Test File

```bash
mkdir -p ~/archive_test/documents
echo "Original content" > ~/archive_test/documents/file1.txt
ls -i ~/archive_test/documents/file1.txt
```

The file was created in a controlled directory and its inode number was displayed.

### 33B. Create and Test a Hard Link

```bash
ln ~/archive_test/documents/file1.txt ~/archive_test/documents/file1_hardlink.txt
ls -i ~/archive_test/documents/file1*
echo "New content" >> ~/archive_test/documents/file1.txt
cat ~/archive_test/documents/file1_hardlink.txt
```

Both names displayed the same inode. Content appended through one pathname was visible through the other because both directory entries referenced the same inode and data.

### 33C. Create and Test a Symbolic Link

```bash
ln -s ~/archive_test/documents/file1.txt ~/archive_test/documents/file1_symlink.txt
ls -il ~/archive_test/documents/file1*
rm ~/archive_test/documents/file1.txt
cat ~/archive_test/documents/file1_symlink.txt
cat ~/archive_test/documents/file1_hardlink.txt
ls -l ~/archive_test/documents/
```

The symbolic link had its own inode and stored a path to the target. Removing the original pathname broke the symbolic link, while the hard link remained usable.

<img width="1366" height="662" alt="Fig01_CLI_HardLinkCreation_InodeVerification png" src="https://github.com/user-attachments/assets/6e4a9a3a-0702-4004-a7f3-12f3c2dcb6cf" />

<img width="1366" height="662" alt="Fig02_CLI_SymbolicLinkCreation_BrokenTarget png" src="https://github.com/user-attachments/assets/a1eab126-bfec-4b0d-95d1-0d4ffe416a3c" />


---

## Results and Findings

A filename is a directory entry rather than the file data itself. Hard links are additional names for the same inode, while symbolic links are independent files containing a target path. Data remains allocated while at least one hard link still references the inode.

---

## Challenges and Solutions

| Challenge | Resolution |
|---|---|
| The symbolic link failed after the original pathname was removed. | This was expected because the stored target path no longer resolved. The surviving hard link was used to confirm that the inode data remained available. |

---

## Screenshot Reference

Screenshot filenames in this rewritten report are professional placeholders derived from the embedded evidence in the source document. Rename exported images to match these links, or update the links to the filenames you select.

---

## Recommendations

- Use `ls -li` to verify inode relationships.
- Prefer symbolic links when linking across filesystems or to directories.
- Remember that hard links normally cannot cross filesystem boundaries.
- Preserve link metadata during forensic collection.

---

## Conclusion

Lab 33 developed practical competency in filesystem links. The work combined controlled execution, verification, and professional documentation, supporting future Linux administration, incident-response, and digital-forensics activities.

---

## Safety and Evidence Handling

All activities were completed in an authorised virtual laboratory. Potentially disruptive operations were limited to test data, loopback images, or controlled system administration tasks. Commands and screenshots were retained to support reproducibility.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem Links, Hardware, Boot Process, Packages and Month 2 Assessment.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
