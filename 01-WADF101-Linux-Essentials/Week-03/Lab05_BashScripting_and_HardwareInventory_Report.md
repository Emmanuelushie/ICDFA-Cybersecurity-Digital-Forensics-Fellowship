# Lab 5: Bash Scripting and Hardware Inventory

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | Linux Essentials: WADF-2026-M01 |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 03 |
| Lab | Lab 5: Bash Scripting and Hardware Inventory |
| Date of Submission | 09/07/2026 |

---

## Executive Summary

This report documents the practical activities completed in Lab 5 of the Week 03 Linux Essentials module. The lab introduced reusable Bash scripting, defensive input validation, loop-based automation, exit status codes, and read-only hardware inventory collection.

Two executable scripts were created. The first used variables and command substitution to produce a system greeting report containing the current user, hostname, date, and working directory. The second accepted a project name, rejected blank input and forward slashes, and used a `for` loop to create three note files. Both success and controlled failure paths were tested. A read-only inventory was then collected for the virtual machine's CPU, memory, storage, network interfaces, mounted filesystems, and virtualisation environment. These activities provide a foundation for repeatable evidence collection, secure scripting, and system baselining.

---

## Objectives

- Write a reusable Bash script using a shebang, comments, variables, and command substitution.
- Grant and verify execute permission on a script.
- Capture script output with `tee` for review and evidence retention.
- Accept user input and validate it before using it in a filesystem path.
- Use an `if` statement and a `for` loop to control script behaviour.
- Test successful and failed execution paths and interpret their exit codes.
- Collect a read-only inventory of CPU, memory, storage, network, mounted filesystems, and virtualisation context.
- Save combined inventory output to a reusable text report.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled environment used for all scripting and inventory activities.
- **Bash shell:** Scripting language and command-line interface.
- **nano:** Terminal editor used to create the scripts.
- **`chmod`:** Granted execute permission to script owners.
- **`nl`:** Displayed script content with line numbers.
- **`tee`:** Displayed output and saved it to a file simultaneously.
- **`lscpu` and `free`:** Collected CPU and memory information.
- **`lsblk` and `df`:** Collected block-device and filesystem-capacity information.
- **`ip`:** Displayed network interfaces and assigned addresses.
- **`systemd-detect-virt`:** Identified the virtualisation environment.

---

## Workspace and Evidence Storage

```text
~/wadf-labs/week3/scripts/
~/wadf-labs/week3/output/
~/wadf-labs/week3/generated-notes/
/media/sf_ICDFA/Week03/Screenshots/
```

---

## Methodology

### 5A. Creating a Reusable Bash Script

A scripts directory and output directory were created before writing a reusable system-information script.

#### Step 1: Create and enter the workspace

```bash
mkdir -p ~/wadf-labs/week3/{scripts,output}
cd ~/wadf-labs/week3/scripts
nano system_greeting.sh
```

Brace expansion created both required directories in one command. The script was then written with `nano`.

#### Step 2: Write the script

```bash
#!/usr/bin/env bash
# Script: system_greeting.sh
# Purpose: Display a system greeting with user, host and date
# Author: Emmanuel Adie Ushie

CURRENT_USER=$(whoami)
HOSTNAME_VAL=$(hostname)
CURRENT_DATE=$(date)
WORKING_DIR=$(pwd)

echo "==============================="
echo " SYSTEM GREETING REPORT"
echo "==============================="
echo "User     : $CURRENT_USER"
echo "Host     : $HOSTNAME_VAL"
echo "Date     : $CURRENT_DATE"
echo "Location : $WORKING_DIR"
echo "==============================="
```

The shebang selected Bash through the environment. Command substitution captured live command output in variables, which were then referenced in the formatted report.

#### Step 3: Review the script with line numbers

```bash
nl -ba system_greeting.sh
```

The `-b a` options numbered all lines, including blank lines, making the script easier to review and reference.

#### Step 4: Grant and verify execute permission

```bash
chmod u+x system_greeting.sh
ls -l system_greeting.sh
```

`chmod u+x` added execute permission for the file owner. `ls -l` confirmed the resulting permission state.

#### Step 5: Execute the script and capture its output

```bash
./system_greeting.sh | tee ~/wadf-labs/week3/output/greeting_report.txt
```

The script ran from the current directory. `tee` displayed the report and saved an identical copy to `greeting_report.txt`.

**Screenshot Filename:** `Fig01_CLI_ScriptCreation_NanoSystemGreeting_NlBa.png`

<img width="1919" height="1047" alt="Fig01_CLI_ScriptCreation_NanoSystemGreeting_NlBa png" src="https://github.com/user-attachments/assets/552c13b3-47c9-4ee7-a987-63e8e6188143" />


**Screenshot Filename:** `Fig02_CLI_ScriptPermissionsAndExecution_ChmodUx_TeeOutput.png`

<img width="1916" height="753" alt="Fig02_CLI_ScriptPermissionsAndExecution_ChmodUx_TeeOutput png" src="https://github.com/user-attachments/assets/791fe7b2-1fb2-4636-b884-3f7ff2f0b64b" />


### 5B. Processing Input, Conditions, and Loops

A second script was created to accept a project name, apply basic safety validation, and generate three numbered note files.

#### Script content

```bash
#!/usr/bin/env bash
# Script: create_notes.sh
# Purpose: Accept a project name and create three note files
# Author: Emmanuel Adie Ushie

read -rp "Enter project name: " project_name

if [[ -z "$project_name" || "$project_name" == */* ]]; then
    echo "Error: project name must not be blank or contain '/'." >&2
    exit 1
fi

TARGET_DIR=~/wadf-labs/week3/generated-notes/"$project_name"
mkdir -p "$TARGET_DIR"

for i in 1 2 3; do
    touch "$TARGET_DIR/note_${i}.txt"
    echo "Note $i created for project: $project_name" > "$TARGET_DIR/note_${i}.txt"
done

echo "Success: 3 notes created in $TARGET_DIR"
exit 0
```

`read -r` prevented backslash interpretation, while `-p` displayed the prompt. The condition rejected blank values and values containing `/`. Quoting the variable protected spaces from unwanted word splitting. Valid input triggered directory creation and a three-iteration loop.

#### Grant execute permission

```bash
chmod u+x create_notes.sh
```

#### Test the successful path

```bash
./create_notes.sh
echo $?
```

A valid project name produced three files and returned exit status `0`, indicating success.

#### Test the controlled failure path

```bash
./create_notes.sh
echo $?
```

Pressing Enter without a project name triggered the validation rule. The error was written to standard error and the script returned exit status `1`.

#### Verify the generated files

```bash
find ~/wadf-labs/week3/generated-notes -maxdepth 2 -type f 2>/dev/null | sort
```

This read-only command confirmed that the loop created the expected note files.

**Screenshot Filename:** `Fig03_CLI_InputValidationScript_CreateNotes_SuccessAndFailure.png`

<img width="1366" height="662" alt="Fig03_CLI_InputValidationScript_CreateNotes_SuccessAndFailure png`" src="https://github.com/user-attachments/assets/18fd53d2-32ff-40b5-a733-75f0903a5007" />

---

### 5C. Collecting a Read-Only Hardware and OS Inventory

A baseline inventory was collected without changing the virtual machine's configuration.

```bash
lscpu | head -n 25
free -h
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
df -hT
ip -br link
ip -br addr
systemd-detect-virt 2>/dev/null || true
```

These commands reported processor details, memory usage, block devices, mounted filesystem capacity, network interfaces, assigned addresses, and the detected virtualisation platform. `|| true` ensured that the final command would not interrupt a larger collection sequence if no virtualisation type was returned.

#### Save the combined inventory

```bash
{
    lscpu | head -n 25
    free -h
    lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
    df -hT
    ip -br link
    ip -br addr
    systemd-detect-virt 2>/dev/null || true
} > ~/wadf-labs/week3/output/system_inventory.txt
```

The grouped command redirected all standard output into one inventory report, producing a reusable system baseline.

**Screenshot Filename:** `Fig04_CLI_HardwareInventory_Lscpu_FreeH_LsblkO.png`

<img width="1366" height="662" alt="Fig04_CLI_HardwareInventory_Lscpu_FreeH_LsblkO png" src="https://github.com/user-attachments/assets/aa8106e4-5f42-4f96-ba32-da20c8ed1312" />

**Screenshot Filename:** `Fig05_CLI_DiskAndNetworkInventory_DfHT_IpBrLink.png`

<img width="1366" height="662" alt="Fig05_CLI_DiskAndNetworkInventory_DfHT_IpBrLink png" src="https://github.com/user-attachments/assets/a8e62d6c-a491-4835-a93e-48bacabd742d" />


**Screenshot Filename:** `Fig06_CLI_VirtualizationDetection_SystemdDetectVirt_InventoryFile.png`

<img width="1366" height="662" alt="Fig06_CLI_VirtualizationDetection_SystemdDetectVirt_InventoryFile png" src="https://github.com/user-attachments/assets/43b13b83-9805-48e7-b0a3-9436166e47ae" />

---

## Results and Findings

The first script demonstrated the standard components of a reusable Bash program: a shebang, documentation comments, variables, command substitution, and formatted output. Granting execute permission showed that file ownership alone does not permit execution. The output captured with `tee` provided visible confirmation and a persistent evidence record.

The second script demonstrated defensive automation. Blank input and forward slashes were rejected before a directory path was constructed. Successful execution returned `0`, while rejected input returned `1`, allowing other commands or automation systems to interpret the result. The `for` loop reduced repetition and created a consistent set of files.

The inventory activity produced a consolidated snapshot of the VM's hardware and operating context. Because the commands were read-only, they were suitable for baseline collection without intentionally changing system configuration.

---

## Challenges and Solutions

| Challenge | Cause | Resolution |
|---|---|---|
| `create_notes.sh` returned `Permission denied`. | Execute permission had not been granted. | `chmod u+x create_notes.sh` was run before executing the script again. |
| The `tee` command reported that the output path did not exist. | A directory had been mistyped as `outpur` instead of `output`. | The directory names were checked with `ls`, the correct `output` directory was created, the empty typo directory was removed with `rmdir`, and the command was rerun successfully. |

---

## Screenshot Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 1 | `Fig01_CLI_ScriptCreation_NanoSystemGreeting_NlBa.png` | 5A |
| Fig 2 | `Fig02_CLI_ScriptPermissionsAndExecution_ChmodUx_TeeOutput.png` | 5A |
| Fig 3 | `Fig03_CLI_InputValidationScript_CreateNotes_SuccessAndFailure.png` | 5B |
| Fig 4 | `Fig04_CLI_HardwareInventory_Lscpu_FreeH_LsblkO.png` | 5C |
| Fig 5 | `Fig05_CLI_DiskAndNetworkInventory_DfHT_IpBrLink.png` | 5C |
| Fig 6 | `Fig06_CLI_VirtualizationDetection_SystemdDetectVirt_InventoryFile.png` | 5C |


---

## Recommendations

- Grant only the execute permissions required for a script and verify them with `ls -l`.
- Enable strict Bash options such as `set -euo pipefail` in future scripts after understanding their behaviour.
- Validate input before using it in filenames, commands, or directory paths.
- Use clear exit codes and write errors to standard error with `>&2`.
- Quote variable expansions to reduce unwanted word splitting and pathname expansion.
- Check destination directories before redirecting or capturing output.
- Add clear section headings and timestamps to future inventory reports.

---

## Conclusion

Lab 5 established a practical foundation in Bash automation and system baselining. Two executable scripts were created and tested, including one that used input validation, conditional logic, a loop, and explicit exit statuses. A consolidated hardware and operating-system inventory was also produced with read-only commands.

These techniques are directly relevant to cybersecurity and digital forensics. Reliable scripts can standardise evidence collection, input validation can prevent unintended filesystem operations, and hardware inventories can provide baselines for later comparison and investigation.

---

## Safety and Evidence Handling

All scripts and generated files were restricted to the authorised Week 03 laboratory workspace. Hardware and operating-system inventory commands were read-only. Both successful and failed script paths were documented, and evidence output was retained without accessing unauthorised systems or data.

---

## Reference

ICDFA. (2026). *WADF-2026-M01 Student Laboratory Workbook: Week 03, Bash Scripting, Hardware Awareness and Data Storage.* International Cybersecurity and Digital Forensics Academy.

