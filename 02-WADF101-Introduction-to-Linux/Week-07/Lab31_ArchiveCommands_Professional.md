# Lab 31: Archive Commands

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 07 |
| Lab | Lab 31: Archive Commands |
| Date of Submission | 06/08/2026 |


---

## Executive Summary

This report documents Lab 31 of Week 07. A controlled directory tree was archived in uncompressed, gzip-compressed, and bzip2-compressed formats. Archive contents were inspected without extraction, complete and selected entries were restored, archive sizes were compared, and time-based file selection was explored.

The lab reinforced the difference between archiving and compression. It also demonstrated that archive path design and extraction destination directly affect portability and restoration safety.

---

## Objectives

- Create and inspect an uncompressed tar archive.
- Produce gzip and bzip2 compressed archives.
- Compare resulting file sizes.
- Restore a complete archive and a selected entry.
- Extract into a specified directory with `-C`.
- Explore time-based archive selection.
- Manage temporary archive files safely.

---

## Tools and Environment

- **`tar`:** Created, listed, and extracted archives.
- **gzip:** Provided fast general-purpose compression.
- **bzip2:** Provided an alternative compression format.
- **`mkdir` and `touch`:** Created controlled test content.
- **`ls -lh`:** Compared archive sizes.
- **`rm`:** Removed an archive after verification.

---

## Workspace and Evidence Storage

```text
~/archive_test/
~/extract_test/
/media/sf_ICDFA/Week07/Screenshots/
```

---

## Methodology

### 31A. Create the Test Structure and Initial Archive

```bash
mkdir -p ~/archive_test/documents ~/archive_test/images
touch ~/archive_test/documents/file1.txt ~/archive_test/documents/file2.txt
touch ~/archive_test/images/photo1.jpg ~/archive_test/images/photo2.jpg
tar -cvf ~/backup.tar -C "$HOME" archive_test
ls -lh ~/backup.tar
tar -tvf ~/backup.tar
```

The workspace contained two subdirectories and four test files. `tar -cvf` created an uncompressed archive. Using `-C "$HOME" archive_test` stored a clean relative top-level path rather than a leading absolute path. `tar -tvf` inspected the archive without extraction.

**Screenshot Filename:** `Fig07_CLI_ArchiveTestSetupAndTarCreation_MkdirTouch_TarCvf.png`

<img width="1366" height="662" alt="Fig07_CLI_ArchiveTestSetupAndTarCreation_MkdirTouch_TarCvf png" src="https://github.com/user-attachments/assets/ad62b7a2-9a8b-4d50-a308-a33448faa041" />


---

### 31B. Apply gzip and bzip2 Compression

```bash
gzip -k ~/backup.tar
tar -czf ~/backup2.tar.gz -C "$HOME" archive_test
tar -cjf ~/backup3.tar.bz2 -C "$HOME" archive_test
ls -lh ~/backup.tar ~/backup.tar.gz ~/backup2.tar.gz ~/backup3.tar.bz2
```

`gzip -k` preserved the original tar file while creating `backup.tar.gz`. The `-z` and `-j` tar options created gzip and bzip2 archives directly. The resulting sizes were compared with `ls -lh`.

> Compression effectiveness depends on file content. Empty test files may not demonstrate a meaningful size advantage, and bzip2 is not guaranteed to be smaller than gzip for every dataset.

**Screenshot Filename:** `Fig08_CLI_ArchiveCompressionFormats_Gzip_TarCzf_TarCjf.png`

<img width="1366" height="662" alt="Fig08_CLI_ArchiveCompressionFormats_Gzip_TarCzf_TarCjf png" src="https://github.com/user-attachments/assets/662a87e9-524a-4165-a9a5-7c7aee96979f" />


---

### 31C. Extract Complete and Selected Content

```bash
mkdir -p ~/extract_test
tar -xzf ~/backup2.tar.gz -C ~/extract_test
tar -xzf ~/backup2.tar.gz -C ~/extract_test archive_test/documents/file1.txt
find ~/extract_test -type f | sort
```

The complete archive was restored to an isolated directory. The selective extraction command named one exact archive member. Listing the archive first with `tar -tf` is recommended to confirm the stored path before selective extraction.

### 31D. Explore Time-Based Selection

```bash
touch ~/archive_reference
touch ~/archive_test/documents/file2.txt
tar -cvf ~/backup_incremental.tar   --newer-mtime-than="$HOME/archive_reference"   -C "$HOME" archive_test

tar -tvf ~/backup_incremental.tar
```

A dedicated reference file provided a clear timestamp boundary. Files modified after that reference were selected. This is a time-based differential selection exercise, not a full stateful incremental-backup system.

```bash
ls -lh ~/backup*.tar*
rm -i ~/backup.tar.gz
```

Archive sizes were reviewed before one disposable archive was removed interactively.

**Screenshot Filename:** `Fig09_CLI_ArchiveExtractionAndIncrementalBackup_TarXvf_NewerMtimeThan.png`

<img width="1366" height="662" alt="Fig09_CLI_ArchiveExtractionAndIncrementalBackup_TarXvf_NewerMtimeThan png" src="https://github.com/user-attachments/assets/9d284e6f-ccff-47b3-9bb9-c742c90d27c0" />


---

## Results and Findings

The lab confirmed that tar packages files and metadata into one archive, while gzip or bzip2 applies compression. Archive inspection before extraction provided visibility into stored paths, and restoration to `extract_test` prevented the source tree from being overwritten.

Using a dedicated reference timestamp improved the clarity of time-based selection. For production incremental backups, GNU tar snapshot files or a dedicated backup system would provide more reliable state tracking than a single modification-time comparison.

---

## Challenges and Solutions

| Challenge | Cause | Resolution |
|---|---|---|
| Archive members contained unwanted path components. | The source was archived without deliberately setting the working directory. | `tar -C "$HOME" archive_test` was used to store a clean relative path. |
| Compression results were difficult to compare. | The test files were empty and contained little compressible data. | Archive commands were verified functionally, and the report notes that meaningful ratio testing requires representative content. |

---

## Screenshot Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 7 | `Fig07_CLI_ArchiveTestSetupAndTarCreation_MkdirTouch_TarCvf.png` | 31A |
| Fig 8 | `Fig08_CLI_ArchiveCompressionFormats_Gzip_TarCzf_TarCjf.png` | 31B |
| Fig 9 | `Fig09_CLI_ArchiveExtractionAndIncrementalBackup_TarXvf_NewerMtimeThan.png` | 31C–31D |

---

## Recommendations

- Inspect archive members with `tar -tf` before extraction.
- Store relative paths by controlling the source directory with `-C`.
- Restore into a separate test directory before relying on an archive.
- Use representative data when comparing compression formats.
- Use a dedicated timestamp or snapshot file for repeatable incremental workflows.
- Use interactive removal when deleting training archives.

---

## Conclusion

Lab 31 developed a complete archive workflow covering creation, inspection, compression, extraction, selective restoration, and time-based selection. The exercise demonstrated that reliable archives depend on correct content, predictable stored paths, and verified restoration.

---

## Safety and Evidence Handling

All archives contained controlled test data. Extraction occurred in a separate directory, and deletion was limited to a disposable archive after review.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 7 Practical Labs: Labs 29–32, Standard Text Streams, Processes, Archives and File Permissions.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
3. GNU Project. (n.d.). *GNU tar Manual.* https://www.gnu.org/software/tar/manual/
