# Lab 8: User Administration and Capstone Challenges

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | Linux Essentials: WADF-2026-M01 |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 04: Month 1 Assessment |
| Lab | Lab 8: User Administration and Capstone Challenges |
| Date of Submission | 16/07/2026 |

---

## Executive Summary

This report documents the practical activities completed in Lab 8 of the Linux Essentials module. The capstone lab integrated Linux user and group administration, permission management, defensive Bash scripting, portable archiving, and authentication-log analysis within a controlled virtual laboratory.

A laboratory group and three user accounts were created and verified before a protected shared directory was configured with setgid permissions. The exercise was then extended into a department-based access-control structure containing three groups, nine user accounts, protected collaboration directories, and confidential files. A reusable Bash onboarding script was developed to validate privileges and user input, prevent duplicate records, create an account and group, and apply controlled directory permissions with distinct exit codes. Seven simulated log files were archived with portable relative paths and restored into a separate directory. Finally, a controlled authentication log was processed with regular expressions, pipelines, threshold filtering, output redirection, and separate error capture.

These activities demonstrate practical Linux administration and security techniques that support controlled access, repeatable automation, evidence preservation, and digital-forensics investigation.

---

## Objectives

The objectives of this lab were to:

- Create and verify Linux users and groups in an authorised laboratory environment.
- Configure shared directories with appropriate ownership and numeric permissions.
- Apply and explain setgid and sticky-bit behaviour.
- Protect confidential files according to department access requirements.
- Develop a reusable Bash user-onboarding script with privilege and input validation.
- Use distinct exit codes to communicate successful and failed script outcomes.
- Create a portable tar archive containing controlled log files.
- Inspect archive contents and verify restoration in a separate directory.
- Analyse a simulated authentication log with regular expressions and pipelines.
- Create structured investigation outputs without modifying the source log.
- Capture standard error separately from normal command output.

---

## Tools and Environment

- **Kali Linux virtual machine:** Provided the controlled environment for all administrative and analytical activities.
- **VirtualBox snapshots:** Provided a recovery point before privileged system changes.
- **`groupadd` and `useradd`:** Created Linux groups and user accounts.
- **`getent` and `id`:** Verified account, group, and identity information.
- **`mkdir`, `chown`, and `chmod`:** Created directories and applied ownership and permission controls.
- **`find` and `ls`:** Verified directory structures, owners, groups, and permission modes.
- **Bash:** Implemented the reusable onboarding script and automated file creation.
- **`tar`:** Created, inspected, and restored a portable archive.
- **`grep -E`, `cut`, `sort`, `uniq`, and `awk`:** Filtered, extracted, counted, ranked, and thresholded authentication events.
- **`tee`, `>`, `>>`, and `2>`:** Created files and controlled standard output and standard error.

---

## Workspace and Evidence Storage

```text
~/wadf-labs/week4/scripts/
~/wadf-challenge/
~/wadf-challenge/log-source/
~/archive/
~/backup/
/srv/wadf-practice/
/srv/wadf-departments/
/srv/wadf-userdirs/
```

Screenshot evidence is stored in:

```text
Screenshots/
```

---

## Methodology

### 8A. User, Group, Ownership, and Permission Foundations

A VirtualBox snapshot named `before-week04-lab8` was created before privileged changes were made. This provided a recovery point if an administrative error affected the laboratory environment.

#### Step 1: Create the laboratory group and accounts

```bash
sudo groupadd labteam
sudo useradd -m -s /bin/bash -g labteam ushie
sudo useradd -m -s /bin/bash -g labteam Abbas
sudo useradd -m -s /bin/bash -g labteam Jake
```

The commands created one laboratory group and three user accounts. The `-m` option created each home directory, `-s /bin/bash` assigned Bash as the login shell, and `-g labteam` selected `labteam` as the primary group.

> Linux account names are case-sensitive. Lowercase usernames are generally preferable for consistency and portability, although this exercise retained the names used in the captured evidence.

#### Step 2: Verify the accounts and group

```bash
id ushie
id Abbas
id Jake
getent passwd ushie
getent group labteam
```

`id` displayed each account's user and group identifiers. `getent` queried the configured account databases and confirmed the passwd and group records.

#### Step 3: Create and secure the shared directory

```bash
sudo mkdir -p /srv/wadf-practice/labteam
sudo chown ushie:labteam /srv/wadf-practice/labteam
sudo chmod 2770 /srv/wadf-practice/labteam
ls -ld /srv/wadf-practice/labteam
```

The directory was assigned to user `ushie` and group `labteam`. Mode `2770` grants full access to the owner and group, denies access to others, and applies setgid to the directory.

#### Special permission bits

- **Setgid (`2xxx` or `g+s`):** Causes new entries created within the directory to inherit the directory's group, subject to filesystem and application behaviour.
- **Sticky bit (`1xxx` or `+t`):** In a writable directory, restricts deletion or renaming so that users generally cannot remove entries owned by other users unless they own the directory or have sufficient privilege.

**Screenshot Filename:** `Fig09_CLI_LabGroupAndUserSetup_Groupadd_Useradd_Chmod2770.png`

<img width="1366" height="662" alt="Fig09_CLI_LabGroupAndUserSetup_Groupadd_Useradd_Chmod2770 png" src="https://github.com/user-attachments/assets/7a72ea3c-0059-4ed7-bbec-9b198bd08424" />


---

### 8B. Challenge A: Department User Management and Confidential Data

Three department groups, nine accounts, protected collaboration directories, and confidential files were created.

#### Step 1: Create the department namespace

```bash
sudo mkdir -p /srv/wadf-departments/{engineering,sales,is}
```

Brace expansion created the three department directories in one operation.

#### Step 2: Create the department groups

```bash
sudo groupadd engineering
sudo groupadd sales
sudo groupadd is
```

Separate groups provided the basis for department-level access control.

#### Step 3: Create department accounts

```bash
sudo useradd -m -s /bin/bash -g engineering eng_admin
sudo useradd -m -s /bin/bash -g engineering eng_user1
sudo useradd -m -s /bin/bash -g engineering eng_user2

sudo useradd -m -s /bin/bash -g sales sales_admin
sudo useradd -m -s /bin/bash -g sales sales_user1
sudo useradd -m -s /bin/bash -g sales sales_user2

sudo useradd -m -s /bin/bash -g is is_admin
sudo useradd -m -s /bin/bash -g is is_user1
sudo useradd -m -s /bin/bash -g is is_user2
```

Each account received a home directory, Bash login shell, and department primary group.

#### Step 4: Apply directory ownership and permissions

```bash
sudo chown eng_admin:engineering /srv/wadf-departments/engineering
sudo chown sales_admin:sales /srv/wadf-departments/sales
sudo chown is_admin:is /srv/wadf-departments/is

sudo chmod 3770 /srv/wadf-departments/engineering
sudo chmod 3770 /srv/wadf-departments/sales
sudo chmod 3770 /srv/wadf-departments/is
```

Mode `3770` combines setgid (`2000`) and sticky bit (`1000`) with full owner and group permissions (`770`). Other users receive no permissions. Setgid supports group inheritance, while the sticky bit adds deletion and rename restrictions within the shared writable directories.

#### Step 5: Create and protect confidential files

```bash
echo "This file contains confidential information for the department." \
  | sudo tee /srv/wadf-departments/engineering/confidential.txt
sudo chown eng_admin:engineering /srv/wadf-departments/engineering/confidential.txt
sudo chmod 640 /srv/wadf-departments/engineering/confidential.txt
```

The same pattern was applied to the `sales` and `is` directories with their respective administrators and groups. Mode `640` grants read and write permissions to the owner, read permission to the group, and no permission to others.

#### Step 6: Verify the complete structure

```bash
sudo find /srv/wadf-departments -maxdepth 2 \
  -printf '%M %u:%g %p\n' | sort
id eng_user1
getent group engineering
```

Equivalent checks were completed for the other departments and accounts. The `find -printf` output provided a consolidated view of permission strings, ownership, group assignment, and paths.

**Screenshot Filename:** `Fig10_CLI_DepartmentNamespaceAndGroups_Mkdir_Groupadd.png`

<img width="1366" height="662" alt="Fig10_CLI_DepartmentNamespaceAndGroups_Mkdir_Groupadd png" src="https://github.com/user-attachments/assets/490afcca-55c5-4a3a-813a-1caf80db7d5b" />


**Screenshot Filename:** `Fig11_CLI_DepartmentUserCreation_UseraddEngSalesIs.png`

<img width="1366" height="662" alt="Fig11_CLI_DepartmentUserCreation_UseraddEngSalesIs png" src="https://github.com/user-attachments/assets/c0869d97-09e4-4f8e-ae44-64d9fb0190de" />

**Screenshot Filename:** `Fig12_CLI_DepartmentOwnershipAndPermissions_Chown_Chmod3770.png`

<img width="1366" height="662" alt="Fig12_CLI_DepartmentOwnershipAndPermissions_Chown_Chmod3770 png" src="https://github.com/user-attachments/assets/d342bce8-5e88-48c1-a1ff-280a23203ff4" />


**Screenshot Filename:** `Fig13_CLI_DepartmentDirectoryVerification_LsLd.png`

<img width="1366" height="662" alt="Fig13_CLI_DepartmentDirectoryVerification_LsLd png" src="https://github.com/user-attachments/assets/4b46c265-f16a-4568-959c-b5d0224f8a8b" />

**Screenshot Filename:** `Fig14_CLI_ConfidentialFileCreation_TeeChownChmod640.png`

<img width="1366" height="662" alt="Fig14_CLI_ConfidentialFileCreation_TeeChownChmod640 png" src="https://github.com/user-attachments/assets/ef5a69a5-93f0-4de1-a545-9f91ebc1ed19" />

**Screenshot Filename:** `Fig15_CLI_ConfidentialFilePermissionCheck_LsL.png`

<img width="1366" height="662" alt="Fig15_CLI_ConfidentialFilePermissionCheck_LsL png" src="https://github.com/user-attachments/assets/9fa78ca2-7fc4-4eca-9254-1e5f9ab22ed4" />


**Screenshot Filename:** `Fig16_CLI_FullDepartmentVerification_FindPrintf_Getent.png`

<img width="1366" height="662" alt="Fig16_CLI_FullDepartmentVerification_FindPrintf_Getent png" src="https://github.com/user-attachments/assets/d68bcff3-c996-446e-b8ff-39d06bda976d" />


---

### 8C. Challenge B: Parameterised Bash User-Onboarding Script

A reusable script was written to validate privilege and input, prevent duplicate account records, create a group and user, and configure a protected directory.

#### Script content

```bash
#!/usr/bin/env bash
# Script: onboard_user.sh
# Purpose: Safely create a new user and group with a protected directory
# Author: Emmanuel Adie Ushie

if [[ $EUID -ne 0 ]]; then
    echo "ERROR: Run with sudo." >&2
    exit 1
fi

read -rp "Enter new group name: " groupname
if [[ -z "$groupname" ]]; then
    echo "ERROR: Group name cannot be blank." >&2
    exit 1
fi

if getent group "$groupname" &>/dev/null; then
    echo "ERROR: Group $groupname already exists." >&2
    exit 2
fi

read -rp "Enter new username: " username
if [[ -z "$username" ]]; then
    echo "ERROR: Username cannot be blank." >&2
    exit 1
fi

if getent passwd "$username" &>/dev/null; then
    echo "ERROR: User $username already exists." >&2
    exit 3
fi

groupadd "$groupname" || exit 4
useradd -m -s /bin/bash -g "$groupname" "$username" || exit 5

USERDIR="/srv/wadf-userdirs/$username"
mkdir -p "$USERDIR" || exit 6
chown "$username:$groupname" "$USERDIR" || exit 7
chmod 3770 "$USERDIR" || exit 8

echo "SUCCESS: User $username in group $groupname created."
ls -ld "$USERDIR"
exit 0
```

The script checked for root privilege, rejected blank values, checked for duplicate database entries, quoted variable expansions, and assigned distinct exit codes to critical failure conditions. Error messages were directed to standard error.

#### Prepare and execute the script

```bash
mkdir -p ~/wadf-labs/week4/scripts
chmod u+x ~/wadf-labs/week4/scripts/onboard_user.sh
cd ~/wadf-labs/week4/scripts
sudo ./onboard_user.sh
echo $?
```

The script was tested for successful creation, duplicate group detection, and duplicate user detection. Exit codes `0`, `2`, and `3` distinguished these outcomes.

> Because the script creates the group before testing whether the username already exists, a duplicate-user failure can leave the newly created group behind. A production version should validate both proposed names before making any change, or implement cleanup when a later step fails.

**Screenshot Filename:** `Fig17_CLI_OnboardScriptSuccessRun_SudoOnboardUser.png`

<img width="1366" height="662" alt="Fig17_CLI_OnboardScriptSuccessRun_SudoOnboardUser png" src="https://github.com/user-attachments/assets/872f3cf2-b911-4151-9c43-3175de5a1f03" />


**Screenshot Filename:** `Fig18_CLI_OnboardScriptDuplicateGroupTest_ExitCode2.png`

<img width="1366" height="662" alt="Fig18_CLI_OnboardScriptDuplicateGroupTest_ExitCode2 png" src="https://github.com/user-attachments/assets/b8c21fc5-8171-44b8-a7d2-f968bca89767" />


**Screenshot Filename:** `Fig19_CLI_OnboardScriptDuplicateUserTest_ExitCode3.png`

<img width="1366" height="662" alt="Fig19_CLI_OnboardScriptDuplicateUserTest_ExitCode3 png" src="https://github.com/user-attachments/assets/e3272752-3532-482d-9df1-fa08da237a5d" />

---

### 8D. Challenge C: Controlled Log File Archiving

Seven simulated log files were created, archived with portable relative paths, inspected, and restored into a separate directory.

#### Step 1: Create the directories and controlled log files

```bash
mkdir -p ~/wadf-challenge/log-source ~/archive ~/backup

for f in alternatives auth bootstrap cron dpkg kern mail; do
    printf 'Controlled lab evidence for %s.log\n' "$f" \
      > ~/wadf-challenge/log-source/${f}.log
done

ls -1 ~/wadf-challenge/log-source
```

The loop created one controlled file for each specified log name.

#### Step 2: Create and inspect the archive

```bash
tar -cvf ~/archive/log.tar -C ~/wadf-challenge/log-source .
tar -tf ~/archive/log.tar
```

The `-C` option changed to the source directory before archiving. As a result, the archive stored relative entries instead of unnecessary host-specific absolute paths. `tar -tf` listed the archive without extracting it.

#### Step 3: Restore and verify the archive

```bash
tar -xvf ~/archive/log.tar -C ~/backup
ls -1 ~/backup
```

The archive was extracted into a separate directory, and the restored names were reviewed to confirm that all seven files were present.

**Screenshot Filename:** `Fig20_CLI_FakeLogFileCreation_ForLoop_LsSource.png`

<img width="1366" height="662" alt="Fig20_CLI_FakeLogFileCreation_ForLoop_LsSource png" src="https://github.com/user-attachments/assets/8dd54980-0273-41d5-ac50-0520a83f3e16" />

**Screenshot Filename:** `Fig21_CLI_TarArchiveCreationWithCFlag_TarCvf.png`

<img width="1366" height="662" alt="Fig21_CLI_TarArchiveCreationWithCFlag_TarCvf png" src="https://github.com/user-attachments/assets/92cde48d-ff2c-4373-a1ae-e7d155239faf" />


**Screenshot Filename:** `Fig22_CLI_ArchiveContentsListing_TarTf.png`

<img width="1366" height="662" alt="Fig22_CLI_ArchiveContentsListing_TarTf png" src="https://github.com/user-attachments/assets/46ecab0e-3970-4386-90f1-f601ad5f3971" />


**Screenshot Filename:** `Fig23_CLI_ArchiveRestoreToBackup_TarXvf.png`

<img width="1366" height="662" alt="Fig23_CLI_ArchiveRestoreToBackup_TarXvf png" src="https://github.com/user-attachments/assets/f74ed3aa-e2d2-4c99-a8f9-48aa0e5378dd" />


**Screenshot Filename:** `Fig24_CLI_RestoreVerification_LsBackup.png`

<img width="1366" height="662" alt="Fig24_CLI_RestoreVerification_LsBackup png" src="https://github.com/user-attachments/assets/e68dc29c-3383-4e6c-ad26-1a6182886334" />


---

### 8E. Challenge D: Pipes, Redirection, and Regular-Expression Investigation

A controlled authentication log containing ten events, including seven failures and three successes, was analysed to produce investigation files while preserving the source data.

#### Step 1: Extract failed authentication events

```bash
grep -E "AUTH FAILED" ~/wadf-challenge/auth-events.log \
  > ~/wadf-challenge/failed_events.txt
cat ~/wadf-challenge/failed_events.txt
```

The command selected only failed authentication records and wrote them to a new file.

#### Step 2: Count failures by source IP address

```bash
grep -E "AUTH FAILED" ~/wadf-challenge/auth-events.log \
  | grep -oE 'src=[^ ]+' \
  | cut -d= -f2 \
  | sort \
  | uniq -c \
  | sort -rn \
  > ~/wadf-challenge/failed_ip_summary.txt
```

The recorded result contained three failures from `198.51.100.23`, three from `203.0.113.57`, and one from `192.0.2.99`.

#### Step 3: Flag addresses meeting the investigation threshold

```bash
grep -E "AUTH FAILED" ~/wadf-challenge/auth-events.log \
  | grep -oE 'src=[^ ]+' \
  | cut -d= -f2 \
  | sort \
  | uniq -c \
  | awk '$1 >= 3 {print $2}' \
  > ~/wadf-challenge/suspicious_ips.txt

date >> ~/wadf-challenge/suspicious_ips.txt
```

The threshold identified `198.51.100.23` and `203.0.113.57`. The date was appended with `>>`, preserving the existing results.

#### Step 4: Capture a controlled command error

```bash
grep 'FAILED' ~/wadf-challenge/this-file-does-not-exist.log \
  2> ~/wadf-challenge/command_errors.txt
cat ~/wadf-challenge/command_errors.txt
```

The `2>` operator redirected standard error to a dedicated file without mixing it with normal output.

#### Pipeline stages

| Stage | Command | Purpose |
|---|---|---|
| 1 | `grep -E "AUTH FAILED"` | Retains failed authentication records. |
| 2 | `grep -oE 'src=[^ ]+'` | Extracts each complete `src=` field. |
| 3 | `cut -d= -f2` | Removes the field name and retains the address. |
| 4 | `sort` | Places identical addresses next to one another. |
| 5 | `uniq -c` | Counts occurrences of each address. |
| 6 | `sort -rn` or `awk` | Ranks counts or filters entries that meet the threshold. |

> No screenshot filename was supplied for Challenge D. Add captured evidence beneath this section to complete the evidence trail.

---

## Results and Findings

### User and Department Administration

The account-management exercise demonstrated the importance of carrying out privileged operations in the correct order. Groups were created before accounts, paths were created before ownership was assigned, and verification commands confirmed the final configuration. Department directories configured with mode `3770` combined collaborative group access, group inheritance, deletion protection, and denial of access to other users. Confidential files configured with mode `640` allowed department administrators to modify the files while department members received read-only access.

### Defensive Bash Automation

The onboarding script demonstrated how privilege checks, blank-input validation, duplicate detection, quoted variables, standard-error messages, and meaningful exit codes improve administrative automation. Explicit status checks prevented the script from reporting success when an underlying command failed. The exercise also revealed an opportunity for stronger transactional behaviour because partial changes may remain when a later operation fails.

### Portable Archive Handling

The archive exercise confirmed that the paths stored inside an archive affect portability and restoration behaviour. Using `tar -C <source-directory> .` produced relative archive entries. Listing the archive before extraction and restoring it to a separate destination provided evidence that the archive was readable and that the intended files could be recovered without overwriting the source.

### Authentication-Log Investigation

The investigation pipeline transformed controlled authentication records into structured analytical outputs. Filtering isolated failed events, field extraction identified source addresses, counting and sorting produced a frequency summary, and `awk` applied a defined threshold. Separate files preserved derived results and command errors without modifying the source log.

---

## Challenges and Solutions

| Challenge | Cause | Resolution |
|---|---|---|
| Verification returned `Permission denied` after restrictive permissions were applied. | The current account was intentionally excluded by the configured access-control policy. | Authorised `sudo` access was used for administrative verification. |
| `chown` reported an invalid user or missing path. | Ownership was applied before the required account or path existed. | The correct sequence was followed: create the group, create the user, create the path, and then apply ownership. |
| `sudo ./onboard_user.sh` returned `command not found` or could not locate the script. | The command was run from the wrong directory or the script path contained an error. | The exact path was identified, the working directory was corrected, and the script was executed again. |
| The script reported `unexpected token -n`. | The numeric root comparison used incorrect conditional syntax. | The check was corrected to `[[ $EUID -ne 0 ]]`. |
| The script printed success after user creation failed. | A variable reference was incorrect and critical command results were not checked. | The variable reference was corrected and `|| exit <code>` checks were added after privileged operations. |
| `tar` reported conflicting operations during extraction. | Lowercase `-c`, which means create, was confused with uppercase `-C`, which changes directory. | The extraction command was corrected to use uppercase `-C` for the destination directory. |
| The archive restored more than the seven intended files. | The original archive source scope included unintended content. | The archive was recreated from the exact source directory with `tar -cvf ~/archive/log.tar -C ~/wadf-challenge/log-source .`. |
| Copying the script failed because of a misspelled directory. | The source or destination path was typed incorrectly. | `find ~/wadf-labs -name "onboard_user.sh"` was used to locate the exact script path before retrying. |

---

## Screenshot Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 9 | `Fig09_CLI_LabGroupAndUserSetup_Groupadd_Useradd_Chmod2770.png` | 8A |
| Fig 10 | `Fig10_CLI_DepartmentNamespaceAndGroups_Mkdir_Groupadd.png` | 8B |
| Fig 11 | `Fig11_CLI_DepartmentUserCreation_UseraddEngSalesIs.png` | 8B |
| Fig 12 | `Fig12_CLI_DepartmentOwnershipAndPermissions_Chown_Chmod3770.png` | 8B |
| Fig 13 | `Fig13_CLI_DepartmentDirectoryVerification_LsLd.png` | 8B |
| Fig 14 | `Fig14_CLI_ConfidentialFileCreation_TeeChownChmod640.png` | 8B |
| Fig 15 | `Fig15_CLI_ConfidentialFilePermissionCheck_LsL.png` | 8B |
| Fig 16 | `Fig16_CLI_FullDepartmentVerification_FindPrintf_Getent.png` | 8B |
| Fig 17 | `Fig17_CLI_OnboardScriptSuccessRun_SudoOnboardUser.png` | 8C |
| Fig 18 | `Fig18_CLI_OnboardScriptDuplicateGroupTest_ExitCode2.png` | 8C |
| Fig 19 | `Fig19_CLI_OnboardScriptDuplicateUserTest_ExitCode3.png` | 8C |
| Fig 20 | `Fig20_CLI_FakeLogFileCreation_ForLoop_LsSource.png` | 8D |
| Fig 21 | `Fig21_CLI_TarArchiveCreationWithCFlag_TarCvf.png` | 8D |
| Fig 22 | `Fig22_CLI_ArchiveContentsListing_TarTf.png` | 8D |
| Fig 23 | `Fig23_CLI_ArchiveRestoreToBackup_TarXvf.png` | 8D |
| Fig 24 | `Fig24_CLI_RestoreVerification_LsBackup.png` | 8D |

---

## Recommendations

- Create groups before accounts and create filesystem paths before assigning ownership.
- Use consistent lowercase account names unless a documented requirement states otherwise.
- Validate all external input before using it in account names, paths, or commands.
- Quote variable expansions and send error messages to standard error.
- Validate both the proposed group and username before making changes, or implement rollback for partial failures.
- Check the result of every critical privileged operation before displaying success.
- Grant only the permissions required by the access policy and verify them with read-only commands.
- Use `sudo` only for operations that genuinely require elevated privilege.
- Create portable archives with `tar -C <source-directory> .`, inspect their contents, and test restoration separately.
- Preserve source logs and write analysis results to new files.
- Use `>>` only when appending and `2>` when standard error must be recorded separately.
- Add screenshot evidence for Challenge D to complete the investigation evidence trail.

---

## Conclusion

Lab 8 integrated Linux account administration, filesystem security, Bash automation, portable archiving, and log analysis into a single capstone exercise. The activities required careful control of operation order, ownership, permission values, variable expansion, exit codes, archive paths, regular expressions, pipelines, and output streams.

The lab demonstrated that secure administration depends on more than executing commands successfully. Effective practice also requires verification, least-privilege access, defensive input handling, predictable failure behaviour, preservation of source evidence, and reproducible documentation. These capabilities map directly to Linux administration, security operations, incident response, and digital-forensics workflows.

---

## Safety and Evidence Handling

All activities were performed in an authorised Kali Linux virtual laboratory. A VirtualBox snapshot was created before privileged changes. User accounts, groups, directories, permissions, simulated logs, and archives were limited to the designated practice environment.

Source log data was preserved during analysis, and derived findings were written to separate files. Archive restoration was performed in a separate destination to avoid overwriting the source. Elevated privileges were used only for authorised administrative tasks, and the report documents both successful execution and troubleshooting outcomes.

---

## References

1. ICDFA. (2026). *WADF-2026-M01 Student Laboratory Workbook: Week 04, Network Configuration, Linux Security, User Administration and Capstone Challenges.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *useradd(8), groupadd(8), chmod(1), tar(1), grep(1), and awk(1).* https://man7.org/linux/man-pages/
3. ICDFA. (2026). *WADF 101 Modules 1–6.*

