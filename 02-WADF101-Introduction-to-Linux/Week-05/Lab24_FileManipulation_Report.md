# Lab 24: File Manipulation

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 05 |
| Date of Submission | 23/07/2026 |
| Lab | Lab 24 — File Manipulation |

---

## Objectives

- Create files with `touch`, `>`, and `>>`.
- Copy files and directories.
- Move, rename, and safely delete filesystem objects.

---

## Tools Used

- **`touch`, `echo`, `cat`** — create and verify content.
- **`cp`, `mv`, `rm`** — copy, move, rename, and remove.
- **`mkdir`, `ls`** — create and verify directories.

---

## Methodology

### 24A. Creation and Redirection

```bash
cd ~/globbing_test
touch newfile1.txt newfile2.txt
echo "This is content" > myfile.txt
echo "Additional line" >> myfile.txt
cat myfile.txt
```

Created files and demonstrated overwrite versus append redirection.

### 24B. Copying

```bash
cp file1.txt file1_backup.txt
mkdir archive
cp *.txt archive/
mkdir original_dir
touch original_dir/file1.txt original_dir/file2.txt
cp -r original_dir copied_dir
```

Copied a file, wildcard-selected files, and a directory recursively.

### 24C. Moving and Renaming

```bash
mv myfile.txt moved_file.txt
mv newfile*.txt archive/
mv file1_backup.txt file1_backup_old.txt
```

Used `mv` for renaming and movement.

### 24D. Deletion

```bash
rm file2.txt
rm -i file3.txt
rm -r original_dir
```

Removed controlled files and a test directory, using interactive confirmation where appropriate.

**Screenshot:** `Fig15_CLI_FileCreationEvidence_TouchEchoRedirect_AppendRedirect.png`

<img width="1366" height="662" alt="Fig15_CLI_FileCreationEvidence_TouchEchoRedirect_AppendRedirect png" src="https://github.com/user-attachments/assets/e4ed9957-788d-4db2-88a0-fb356bdf364c" />


**Screenshot:** `Fig16_CLI_SingleAndWildcardCopyEvidence_CpBackup_CpArchive.png`

<img width="1366" height="768" alt="Fig16_CLI_SingleAndWildcardCopyEvidence_CpBackup_CpArchive png" src="https://github.com/user-attachments/assets/2f60003e-0e99-4f01-9971-f81e0173182e" />

**Screenshot:** `Fig17_CLI_RecursiveDirectoryCopyEvidence_CpR.png`

<img width="1366" height="662" alt="Fig17_CLI_RecursiveDirectoryCopyEvidence_CpR png" src="https://github.com/user-attachments/assets/2d662e94-066a-4387-b19b-1f04e1fa194f" />


**Screenshot:** `Fig18_CLI_MoveAndRenameEvidence_Mv.png`

<img width="1366" height="662" alt="Fig18_CLI_MoveAndRenameEvidence_Mv png" src="https://github.com/user-attachments/assets/cb858a7f-b7ab-4dff-ad7f-3c97c54d15e3" />


**Screenshot:** `Fig19_CLI_DeletionEvidence_Rm_RmI_RmR.png`

<img width="1366" height="662" alt="Fig19_CLI_DeletionEvidence_Rm_RmI_RmR png" src="https://github.com/user-attachments/assets/4ad93582-779d-49c0-b1b1-a8cf459d58df" />


---

## Analysis and Findings

The lab covered the complete lifecycle of controlled files. The difference between `>` and `>>` is critical because overwrite redirection can destroy data. Recursive copying is required for directories, while `rm` demands careful path verification because deletion is normally immediate.

---

## Challenges and Solutions

| Challenge | Solution |
|---|---|
| `>` can destroy existing content. | Used `>>` when appending was required. |
| A directory cannot be copied without recursion. | Used `cp -r`. |
| Deletion is immediate. | Used `rm -i` and verified paths first. |

---

## Conclusion

Lab 24 developed practical competence in file manipulation. The exercise connected accurate command use, verification, and safe Linux working practices to administration and cybersecurity workflows.

---

## Screenshots Reference

| Figure | Filename | Lab |
|---|---|---|
| Fig 1 | `Fig15_CLI_FileCreationEvidence_TouchEchoRedirect_AppendRedirect.png` | 24 |
| Fig 2 | `Fig16_CLI_SingleAndWildcardCopyEvidence_CpBackup_CpArchive.png` | 24 |
| Fig 3 | `Fig17_CLI_RecursiveDirectoryCopyEvidence_CpR.png` | 24 |
| Fig 4 | `Fig18_CLI_MoveAndRenameEvidence_Mv.png` | 24 |
| Fig 5 | `Fig19_CLI_DeletionEvidence_Rm_RmI_RmR.png` | 24 |

*Place each screenshot inside `Screenshots/` using the exact filename shown above.*

---

## Recommendations

- Verify source and destination paths.
- Preview wildcard matches.
- Use interactive deletion for important files.
- Avoid recursive removal outside controlled directories.

---

## Reference

ICDFA. (2026). *WADF-2026-M02 Week 5 Practical Labs: Labs 21–24, Using the Shell and File Globbing.* International Cybersecurity and Digital Forensics Academy.

