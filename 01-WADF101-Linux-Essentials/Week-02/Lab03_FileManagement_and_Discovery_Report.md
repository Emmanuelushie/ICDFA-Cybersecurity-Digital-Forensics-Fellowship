# Lab 3: File Management and Discovery

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | Linux Essentials WADF-2026-M01 |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 02 |
| Lab | Lab 3 — File Management and Discovery |
| Date of Submission | 03/06/2026 |

---

## Executive Summary

This report documents the practical activities completed in Lab 3 of the Week 02 Linux Essentials module. The lab focused on building a controlled project workspace, managing files throughout their lifecycle, and locating files efficiently with shell wildcards and the `find` command.

A structured directory hierarchy was created with `mkdir -p` and brace expansion. Files were then created, copied, modified, compared, moved, and safely removed. The lab also demonstrated how wildcards support quick filename matching within a specified directory, while `find` provides recursive searches with filters such as filename, file type, size, and modification date. These activities developed practical file-management habits that are relevant to cybersecurity operations and digital forensics, where accuracy, traceability, and controlled handling are essential.

---

## Objectives

The objectives of this lab were to:

- Build a structured project workspace with a clear directory hierarchy using `mkdir -p`.
- Use brace expansion to create multiple related directories efficiently.
- Practise the complete file lifecycle, including creation, copying, editing, comparison, movement, and safe deletion.
- Use shell wildcards to match files within a specified directory.
- Use `find` to locate files recursively by name, type, size, and modification date.
- Explain the difference between shell wildcard expansion and the deeper search capabilities of `find`.
- Apply safe working practices before performing bulk file operations.

---

## Tools and Environment

- **Kali Linux virtual machine:** Provided the controlled Linux laboratory environment in which all practical exercises were completed.
- **Bash shell:** Served as the command-line interface for directory creation, file management, and file discovery.
- **`mkdir`:** Created the project directory hierarchy.
- **`touch` and `printf`:** Created files and added controlled text content.
- **`cp` and `mv`:** Copied and moved files through the project workflow.
- **`diff`:** Compared the original and edited versions of a file.
- **`rm -i`:** Supported safer deletion by requesting confirmation before removing a file.
- **Shell wildcards:** Matched groups of files based on filename patterns.
- **`find`:** Performed recursive searches using name, type, size, and modification-time criteria.
- **`sort`, `cat`, and `ls`:** Verified directory structures, file contents, and file details.

---

## Workspace and Evidence Storage

The following directories were used to store the Week 02 report files, notes, and screenshot evidence:

```text
/media/sf_ICDFA/Week02/
/media/sf_ICDFA/Week02/Screenshots/
/media/sf_ICDFA/Week02/Notes/
```

The practical project workspace was created under:

```text
~/wadf-labs/week2/project/
```

---

## Methodology

### 3A. Building a Controlled Project Workspace

A structured workspace was created to separate incoming files, active work, completed items, archives, and evidence. This approach supports organised file handling and reduces the likelihood of files being misplaced or modified unintentionally.

#### Step 1: Create the directory hierarchy

```bash
mkdir -p ~/wadf-labs/week2/project/{incoming,working/{day1,day2,day3},completed,archive,evidence}
```

This command created the complete project structure in one operation. The `-p` option created any required parent directories, while brace expansion generated the related subdirectories efficiently.

#### Step 2: Document the purpose of the workspace

```bash
printf "Incoming files, working files, completed items, archives and evidence.\n" > ~/wadf-labs/week2/project/README.txt
```

The command created `README.txt` and recorded a short description of the top-level directories. The single redirection operator, `>`, created the file or replaced its previous contents.

#### Step 3: Verify the directory structure

```bash
find ~/wadf-labs/week2/project -maxdepth 3 -type d | sort
```

This command listed directories to a maximum depth of three levels. Piping the results to `sort` arranged the output alphabetically and made the hierarchy easier to verify.

#### Step 4: Verify the README content

```bash
cat ~/wadf-labs/week2/project/README.txt
```

The `cat` command displayed the file contents and confirmed that the workspace description had been written successfully.

**Screenshot Filename:** `Fig01_CLI_ProjectWorkspaceSetup_MkdirP_PrintfReadme.png`

<img width="1366" height="662" alt="Fig01_CLI_ProjectWorkspaceSetup_MkdirP_PrintfReadme png" src="https://github.com/user-attachments/assets/885da61b-a560-42f7-9843-d59e9b85e3e6" />


**Screenshot Filename:** `Fig02_CLI_ProjectStructureVerification_Find_Cat.png`

<img width="1366" height="662" alt="Fig02_CLI_ProjectStructureVerification_Find_Cat png" src="https://github.com/user-attachments/assets/4f8f3052-7dad-4701-9d0a-e0fe4d2ce43d" />


---

### 3B. Creating, Copying, Editing, Moving, and Removing Files Safely

This activity demonstrated the complete lifecycle of a file. Files were created in the incoming area, copied to a working directory, modified, compared, moved into the completed directory, and safely deleted when no longer required.

#### Step 1: Enter the project workspace

```bash
cd ~/wadf-labs/week2/project
```

Changing to the project directory ensured that the remaining commands operated within the intended controlled workspace.

#### Step 2: Create incoming files

```bash
touch incoming/{client-notes.txt,status.txt,tasks.txt}
```

Brace expansion was used to create three empty text files in a single command.

#### Step 3: Add dated content to the status file

```bash
printf "Reviewed on $(date +%F).\n" >> incoming/status.txt
```

The command appended a review line containing the current date. The `>>` operator was used so that existing content would not be overwritten.

#### Step 4: Copy files to the working directory

```bash
cp incoming/*.txt working/day1/
```

The `*.txt` wildcard matched all text files in `incoming`. The files were copied to `working/day1`, while the original versions remained unchanged.

#### Step 5: Edit the working copy

```bash
printf "Day 1 revision added.\n" >> working/day1/status.txt
```

An additional line was appended to the working copy of `status.txt`, creating a deliberate difference between the original and working versions.

#### Step 6: Compare the original and edited files

```bash
diff -u incoming/status.txt working/day1/status.txt
```

The unified output option, `-u`, displayed the differences between the files with contextual information. This confirmed that only the working copy contained the revision line.

#### Step 7: Move and rename the completed file

```bash
mv working/day1/status.txt completed/status_$(date +%F).txt
```

The edited status file was moved to the `completed` directory and renamed with the current date. Date-based naming improves traceability and version identification.

#### Step 8: Practise safe deletion

```bash
touch working/day1/remove-test.tmp
rm -i working/day1/remove-test.tmp
```

A disposable test file was created and removed with `rm -i`. The interactive option requested confirmation before deletion, reducing the risk of accidental data loss.

**Screenshot Filename:** `Fig03_CLI_FileLifecycle_TouchCopyEdit_Diff.png`

<img width="1366" height="662" alt="Fig03_CLI_FileLifecycle_TouchCopyEdit_Diff png" src="https://github.com/user-attachments/assets/3fde7ffb-49b2-40d0-bb1a-a84237805287" />


**Screenshot Filename:** `Fig04_CLI_FileMoveAndSafeDelete_Mv_RmI.png`

<img width="1366" height="662" alt="Fig04_CLI_FileMoveAndSafeDelete_Mv_RmI png" src="https://github.com/user-attachments/assets/bd31ae52-4d5a-483b-bb52-44c829cbfd72" />


---

### 3C. Using Wildcards and `find` for File Discovery

This activity compared two approaches to file discovery. Shell wildcards provided quick filename matching in a specified directory, while `find` searched recursively and supported additional filters.

#### Step 1: Preview text files with a wildcard

```bash
echo incoming/*.txt
```

The shell expanded `*.txt` into the names of matching text files in the `incoming` directory. This provided a read-only preview of the files that a later bulk command could affect.

#### Step 2: Search recursively by filename

```bash
find . -type f -name "*.txt" | sort
```

Starting from the current directory, `find` searched the complete project tree for regular files whose names ended in `.txt`. The output was sorted alphabetically.

#### Step 3: Create a file for size-based testing

```bash
base64 /dev/urandom | head -c 2048 > working/day2/sample-large.txt
```

This pipeline generated 2,048 characters of test data and saved them to `sample-large.txt`. The file was intentionally made larger than 1 KB for the next search.

#### Step 4: Search by file size

```bash
find . -type f -size +1k
```

The command searched recursively for regular files larger than one kilobyte. The test file created in the previous step satisfied this condition.

#### Step 5: Search by modification date

```bash
find . -type f -daystart -mtime 0
```

This command located regular files modified during the current day, measured from the start of the day rather than from the exact execution time.

#### Step 6: Review detailed file information

```bash
ls -l incoming/*.txt
```

The command displayed permissions, ownership, size, and modification details for all matching text files in `incoming`. It also demonstrated the value of previewing wildcard matches before copying, moving, or deleting files in bulk.

#### Wildcards compared with `find`

A shell wildcard such as `*.txt` is expanded by Bash before the command runs. It is most suitable for quick matching within an explicitly named directory. In contrast, `find` traverses a directory tree recursively and can filter results using criteria such as filename, file type, size, and modification time. Therefore, wildcards are convenient for simple local operations, while `find` is more appropriate for detailed file discovery and investigative work.

**Screenshot Filename:** `Fig05_CLI_WildcardVsFind_EchoGlob_FindName.png`

<img width="1366" height="662" alt="Fig05_CLI_WildcardVsFind_EchoGlob_FindName png" src="https://github.com/user-attachments/assets/f5c3dfa1-4ca4-42ed-8a95-2dd21b25539d" />

**Screenshot Filename:** `Fig06_CLI_FindBySizeAndDate_FindSize_Mtime.png`

<img width="1366" height="662" alt="Fig06_CLI_FindBySizeAndDate_FindSize_Mtime png" src="https://github.com/user-attachments/assets/bc170061-1d8d-405a-a712-cd48d5859031" />


## Results and Findings

The controlled project hierarchy was created successfully and verified with `find`, `sort`, and `cat`. Brace expansion reduced the number of commands required and helped maintain a consistent directory structure.

The file-lifecycle exercise confirmed that files can be managed safely while preserving original copies. The original `status.txt` remained in `incoming`, while the copied version was modified, compared with `diff`, and moved into `completed` with a date-based filename. Interactive deletion also demonstrated a simple safeguard against removing files unintentionally.

The discovery exercise established an important distinction between wildcards and `find`. Wildcards are efficient when the target directory and filename pattern are already known. The `find` command is better suited to recursive discovery because it can combine multiple conditions, including file type, name, size, and modification time. These capabilities are useful in cybersecurity and digital-forensics workflows, where analysts may need to locate relevant files across complex directory trees without modifying them.

---

## Challenges and Solutions

| Challenge | Cause | Resolution |
|---|---|---|
| `diff` initially displayed no differences. | The additional revision line had not yet been written to the working copy. | The `printf` command was run against `working/day1/status.txt`, after which `diff -u` correctly displayed the change. |
| `diff` returned `incoming/status.txt: No such file or directory`. | The command used `incoming`, but the directory in that attempt had been created as `Incoming`. Linux filenames and paths are case-sensitive. | The command path was corrected to match the directory's exact capitalisation. Consistent lowercase directory names were used afterward. |

---

## Screenshot Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 1 | `Fig01_CLI_ProjectWorkspaceSetup_MkdirP_PrintfReadme.png` | 3A |
| Fig 2 | `Fig02_CLI_ProjectStructureVerification_Find_Cat.png` | 3A |
| Fig 3 | `Fig03_CLI_FileLifecycle_TouchCopyEdit_Diff.png` | 3B |
| Fig 4 | `Fig04_CLI_FileMoveAndSafeDelete_Mv_RmI.png` | 3B |
| Fig 5 | `Fig05_CLI_WildcardVsFind_EchoGlob_FindName.png` | 3C |
| Fig 6 | `Fig06_CLI_FindBySizeAndDate_FindSize_Mtime.png` | 3C |

---

## Recommendations

- Use clear and consistent lowercase names for directories to avoid errors caused by Linux case sensitivity.
- Run `echo`, `ls`, or `find` as a read-only preview before using wildcard patterns with `cp`, `mv`, or `rm`.
- Preserve original files in an incoming or evidence directory and make changes only to controlled working copies.
- Use `diff -u` to document differences between original and edited file versions.
- Use date-based filenames where version tracking and traceability are required.
- Prefer `rm -i` when practising deletion or when the effect of a command requires manual verification.
- Use `find` instead of a simple wildcard when recursive searching or filtering by metadata is required.

---

## Conclusion

Lab 3 provided practical experience in organising a Linux workspace and managing files safely throughout their lifecycle. A complete project hierarchy was created, files were processed from incoming to completed status, edited versions were compared against originals, and a disposable file was removed with interactive confirmation.

The lab also demonstrated that wildcards and `find` serve different but complementary purposes. Wildcards provide fast matching in known locations, while `find` supports recursive and condition-based discovery. Together, these skills establish a reliable foundation for system administration, cybersecurity operations, and digital-forensics work, where organised handling and accurate file discovery are essential.

---

## Safety and Evidence Handling

All commands were executed in an authorised Kali Linux virtual laboratory. File creation, modification, movement, and deletion were limited to the designated Week 02 practice workspace. No production systems, third-party infrastructure, or unauthorised data were accessed.

Screenshot evidence was retained to demonstrate command execution and results. Original files were preserved where required, and deletion practice was limited to a clearly identified temporary test file.

---

## Reference

ICDFA. (2026). *WADF-2026-M01 Student Laboratory Workbook — Week 02: Managing Files, Archiving, Compression and Text Processing.* International Cybersecurity and Digital Forensics Academy.

