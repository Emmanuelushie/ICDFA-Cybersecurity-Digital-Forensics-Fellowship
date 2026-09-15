# Lab 4: Archiving, Compression and Text Processing

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | Linux Essentials: WADF-2026-M01 |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 02 |
| Lab | Lab 4: Archiving, Compression and Text Processing |
| Date of Submission | *[Add submission date]* |

---

## Executive Summary

This report documents the practical activities completed in Lab 4 of the Week 02 Linux Essentials module. The lab focused on archiving and compressing project files, generating a cryptographic checksum to support integrity verification, testing archive restoration, and processing simulated log data with standard Linux text utilities.

The `completed` and `evidence` directories were packaged into an uncompressed tar archive and a gzip-compressed archive. The archive contents and file sizes were reviewed, a SHA-256 checksum record was generated, and the compressed archive was extracted into a separate test directory to confirm that its contents could be restored. A simulated access-event log was then examined with `head`, `tail`, `wc`, and `grep`. A command pipeline combining `cut`, `sort`, and `uniq -c` was used to produce a concise user-activity summary. These activities demonstrate practical techniques relevant to system administration, incident response, and digital-forensics evidence handling.

---

## Objectives

The objectives of this lab were to:

- Create a standard tar archive containing completed project files and supporting evidence.
- Create a gzip-compressed version of the archive.
- Inspect archive contents before extraction.
- Compare the sizes of compressed and uncompressed archives.
- Generate a SHA-256 checksum record to support subsequent integrity verification.
- Extract the compressed archive into a separate test directory and confirm a successful restore.
- Use `head`, `tail`, `wc`, and `grep` to inspect and search log data.
- Use `cut`, `sort`, and `uniq` in a pipeline to summarise repeated values.
- Apply `>` and `>>` correctly when creating and appending to output files.
- Document safe and reproducible archive and text-processing procedures.

---

## Tools and Environment

- **Kali Linux virtual machine:** Provided the controlled Linux environment used for all practical activities.
- **Bash shell:** Served as the command-line interface for archive management and text processing.
- **`tar`:** Created, listed, and extracted archive files.
- **gzip through `tar -z`:** Compressed the archived data into `.tar.gz` format.
- **`sha256sum`:** Generated a SHA-256 digest record for the compressed archive.
- **`ls` and `find`:** Displayed archive sizes and verified restored file paths.
- **`head` and `tail`:** Displayed selected lines from the beginning and end of a text file.
- **`wc`:** Counted lines in the simulated log.
- **`grep`:** Selected log entries containing a specified username.
- **`cut`, `sort`, and `uniq`:** Extracted, organised, and counted username values.
- **Shell redirection and pipelines:** Directed command output to files and passed data between utilities.
- **Python HTTP server:** Supported a temporary, authorised file transfer between the virtual machine and host system during troubleshooting.

---

## Workspace and Evidence Storage

The practical work was completed in the following project workspace:

```text
~/wadf-labs/week2/project/
```

Report files, notes, and screenshot evidence were stored in:

```text
/media/sf_ICDFA/Week02/
/media/sf_ICDFA/Week02/Screenshots/
/media/sf_ICDFA/Week02/Notes/
```

---

## Methodology

### 4A. Creating and Validating Archives

The `completed` and `evidence` directories were packaged into tar archives. The contents were inspected, compression was applied, a checksum record was generated, and the compressed archive was restored into an isolated test directory.

#### Step 1: Enter the project workspace

```bash
cd ~/wadf-labs/week2/project
```

Changing to the project directory allowed the archive to be created with relative paths. Relative paths improve portability and avoid embedding unnecessary absolute directory information in the archive.

#### Step 2: Create an uncompressed tar archive

```bash
tar -cvf archive/week2_project.tar completed evidence
```

This command packaged the `completed` and `evidence` directories into `week2_project.tar`:

- `-c` created a new archive.
- `-v` displayed the files being processed.
- `-f` specified the archive filename.

A tar archive combines multiple files and directories into one file but does not apply compression unless a compression option is included.

#### Step 3: Inspect the archive contents

```bash
tar -tf archive/week2_project.tar
```

The `-t` option listed the archived paths without extracting them. This read-only inspection confirmed that the intended directories and files had been included.

#### Step 4: Create a gzip-compressed archive

```bash
tar -czvf archive/week2_project.tar.gz completed evidence
```

The additional `-z` option applied gzip compression while creating the archive. The resulting `.tar.gz` file combined the original directory structure with compression in a single operation.

#### Step 5: Compare archive sizes

```bash
ls -lh archive/week2_project.tar archive/week2_project.tar.gz
```

The `-l` option displayed detailed file information, while `-h` presented file sizes in a human-readable format. This made it possible to compare the compressed and uncompressed archive sizes directly.

#### Step 6: Generate a SHA-256 checksum record

```bash
sha256sum archive/week2_project.tar.gz > archive/week2_project.tar.gz.sha256
```

The command calculated the SHA-256 digest of the compressed archive and saved the result in a companion `.sha256` file. The single `>` operator created or replaced the checksum file. The stored digest can later be compared with a newly calculated digest to detect whether the archive has changed.

#### Step 7: Create a restore-test directory and extract the archive

```bash
mkdir -p restore-test
tar -xzvf archive/week2_project.tar.gz -C restore-test
```

The first command created an isolated restoration directory. The second extracted the compressed archive into that directory:

- `-x` extracted the archive.
- `-z` processed gzip compression.
- `-v` displayed the extracted paths.
- `-f` identified the archive file.
- `-C restore-test` selected the extraction destination.

Using a separate test directory prevented the restored files from overwriting the active project data.

#### Step 8: Verify the restored file paths

```bash
find restore-test -type f | sort
```

This command listed all regular files restored from the archive and sorted the paths alphabetically. The result was reviewed to confirm that the expected files were present after extraction.

**Screenshot Filename:** `Fig07_CLI_TarArchiveCreation_TarCvf_TarTf_TarCzvf.png`

<img width="1366" height="662" alt="Fig07_CLI_TarArchiveCreation_TarCvf_TarTf_TarCzvf png`" src="https://github.com/user-attachments/assets/cbf2156e-65ca-40d6-ae50-2196b81307d3" />

**Screenshot Filename:** `Fig08_CLI_ArchiveIntegrityAndRestore_Sha256sum_TarXzvf.png`

<img width="1366" height="662" alt="`Fig08_CLI_ArchiveIntegrityAndRestore_Sha256sum_TarXzvf png`" src="https://github.com/user-attachments/assets/9c454de7-f026-43bd-ac6f-8bf679c2668f" />


---

### 4B. Text Utilities and Stream Redirection

A simulated access-event log containing timestamps, usernames, and actions was created and analysed entirely from the command line. Individual utilities were used for inspection and searching, while a pipeline produced a saved username-frequency summary.

#### Step 1: Create the simulated log file

```bash
cat > access-events.txt <<'EOF'
...
EOF
```

A quoted heredoc delimiter was used to direct a block of text into `access-events.txt`. The single `>` operator created the file or replaced its previous contents. The ellipsis in this report represents the six event records entered during the laboratory exercise.

#### Step 2: Display the first three records

```bash
head -n 3 access-events.txt
```

This command displayed the first three lines of the file and provided a quick preview of its structure and earliest entries.

#### Step 3: Display the final three records

```bash
tail -n 3 access-events.txt
```

This command displayed the final three lines, which provided a quick view of the most recently listed events in the simulated dataset.

#### Step 4: Count the event records

```bash
wc -l access-events.txt
```

The `-l` option counted newline-terminated lines and confirmed the number of event records stored in the file.

#### Step 5: Search for events associated with a username

```bash
grep "alice" access-events.txt
```

The command returned lines containing the text `alice`, isolating the records associated with that username in the simulated log.

#### Step 6: Produce a username-frequency summary

```bash
cut -d' ' -f3 access-events.txt | sort | uniq -c > user_action_summary.txt
```

This pipeline processed the log in four stages:

1. `cut -d' ' -f3` selected the third space-delimited field from each record.
2. `sort` placed identical usernames next to one another.
3. `uniq -c` counted consecutive occurrences of each username.
4. `>` wrote the summary to `user_action_summary.txt`, replacing any previous contents.

The selected field assumes that the simulated log uses consistent single-space field separation.

#### Step 7: Append the processing date

```bash
date >> user_action_summary.txt
```

The `>>` operator appended the current date and time to the existing summary without removing the username counts.

#### Step 8: Review the completed summary

```bash
cat user_action_summary.txt
```

The completed summary was displayed to confirm that it contained both the username-frequency results and the appended date.

#### Output redirection: `>` compared with `>>`

The single redirection operator, `>`, creates a file or replaces all existing content in that file. The double operator, `>>`, appends new output to the end of a file without removing its current contents. Selecting the correct operator is essential when working with reports, logs, and evidence records because unintended overwriting may destroy previously collected information.

**Screenshot Filename:** `Fig09_CLI_LogFileTextUtilities_Head_Tail_Wc_Grep.png`

<img width="1366" height="662" alt="Fig09_CLI_LogFileTextUtilities_Head_Tail_Wc_Grep png`" src="https://github.com/user-attachments/assets/1f32b8d3-1f8d-4f76-a7a4-20f639dbcb60" />


**Screenshot Filename:** `Fig10_CLI_LogSummaryPipelineAndRedirection_CutSortUniq_DateAppend.png`

<img width="1366" height="662" alt="Fig10_CLI_LogSummaryPipelineAndRedirection_CutSortUniq_DateAppend png" src="https://github.com/user-attachments/assets/54455797-1201-47f0-bfe4-d1936a26ad6e" />


---

## Results and Findings

The archive exercise successfully produced both uncompressed and gzip-compressed versions of the project data. Listing the uncompressed archive before extraction confirmed that the intended paths had been packaged. Comparing the files with `ls -lh` demonstrated the storage effect of gzip compression on the selected dataset.

A SHA-256 digest record was created for the compressed archive. A checksum does not prevent modification, identify who changed a file, or by itself prove malicious tampering. Instead, comparison with a trusted checksum value can reveal that the file contents are no longer identical to the state in which the trusted value was generated. This distinction is important when checksums are used in evidence-handling procedures.

Extraction into `restore-test` confirmed that the archive was readable and that its regular files could be restored without overwriting the active project directories. Reviewing the extracted paths provided an additional check that the expected files were present.

The text-processing exercise showed how small Linux utilities can be combined to analyse structured data efficiently. `head`, `tail`, `wc`, and `grep` supported quick inspection and filtering, while `cut | sort | uniq -c` transformed individual event records into a concise username-frequency summary. The exercise also demonstrated how output redirection can preserve results for later review.

---

## Challenges and Solutions

| Challenge | Cause | Resolution |
|---|---|---|
| The copied `week2_project.tar.gz` archive did not appear on the Windows host through `/media/sf_ICDFA`. | The expected shared-folder transfer was not functioning as intended. | A temporary HTTP transfer method was used within the authorised laboratory environment, and the shared-folder configuration was identified for later review. |
| The host could not access a Python HTTP server through `127.0.0.1:8080`. | `127.0.0.1` on the host refers to the host itself, while `127.0.0.1` inside the virtual machine refers to the guest. The NAT configuration also did not expose the guest service directly to the host through that address. | The VirtualBox network mode was temporarily changed to **Bridged Adapter**. After Kali Linux obtained the address `10.74.253.192`, `python3 -m http.server 8080` was run from the archive directory and the authorised host downloaded the archive from `http://10.74.253.192:8080`. The adapter was returned to NAT mode after the transfer. |

### Lesson Learned

Loopback addresses are local to each operating system instance. A service listening in a virtual machine cannot be reached from the host by entering the host's own `127.0.0.1` address unless appropriate forwarding has been configured. In this controlled exercise, temporary bridged networking placed the guest and host on a reachable network. Network mode changes and temporary servers should be limited to authorised environments, monitored carefully, and reversed immediately after use.

---

## Screenshot Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 7 | `Fig07_CLI_TarArchiveCreation_TarCvf_TarTf_TarCzvf.png` | 4A |
| Fig 8 | `Fig08_CLI_ArchiveIntegrityAndRestore_Sha256sum_TarXzvf.png` | 4A |
| Fig 9 | `Fig09_CLI_LogFileTextUtilities_Head_Tail_Wc_Grep.png` | 4B |
| Fig 10 | `Fig10_CLI_LogSummaryPipelineAndRedirection_CutSortUniq_DateAppend.png` | 4B |

---

## Recommendations

- Generate a SHA-256 checksum record immediately after creating an important archive and protect the trusted checksum from unauthorised modification.
- Verify a saved checksum before relying on or extracting an archive, for example with `sha256sum -c archive/week2_project.tar.gz.sha256` from the project directory.
- List archive contents with `tar -tf` before extraction and restore untrusted or test archives into an isolated directory.
- Compare restored files with the expected source data when stronger restoration assurance is required.
- Use `>` only when intentional creation or replacement is required, and use `>>` when appending to an existing report or log.
- Review output filenames and destination paths before applying redirection.
- Confirm the structure and delimiter of log data before selecting fields with `cut`.
- Prefer robust parsing tools when processing irregular or complex log formats.
- Use shared folders or another approved transfer method where possible. If a temporary network service is necessary, restrict it to the authorised lab, stop it immediately afterward, and restore the original network configuration.

---

## Conclusion

Lab 4 developed practical skills in archive creation, compression, integrity checking, restoration testing, and command-line text processing. Project files were packaged into tar and gzip-compressed archives, a SHA-256 digest record was generated, and the compressed archive was restored into a controlled test directory. These steps demonstrated a reproducible approach to packaging and reviewing collected files.

The log-processing activity showed how standard Linux utilities can be combined to inspect, filter, count, and summarise structured text efficiently. It also reinforced the importance of selecting the correct output-redirection operator to prevent unintended data loss. Together, these techniques provide a useful foundation for Linux administration, security operations, incident response, and digital-forensics workflows.

---

## Safety and Evidence Handling

All activities were completed in an authorised Kali Linux virtual laboratory using project files and simulated log data. Archive creation, extraction, checksum generation, and text processing were limited to the designated Week 02 workspace.

The restoration test used a separate directory to avoid overwriting active files. The temporary HTTP server and bridged network configuration were used only for an authorised transfer between the laboratory virtual machine and its host. The temporary service was stopped and the network adapter was returned to NAT mode after the transfer.

A checksum value should be retained in a trusted location if it is to support later integrity verification. Documentation, timestamps, access controls, and chain-of-custody procedures remain necessary when handling formal digital evidence.

---

## Reference

ICDFA. (2026). *WADF-2026-M01 Student Laboratory Workbook: Week 02, Managing Files, Archiving, Compression and Text Processing.* International Cybersecurity and Digital Forensics Academy.

