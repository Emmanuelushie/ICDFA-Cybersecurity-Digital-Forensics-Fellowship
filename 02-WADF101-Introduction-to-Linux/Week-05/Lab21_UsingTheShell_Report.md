# Lab 21: Using the Shell

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 05 |  
| Lab | Lab 21 — Using the Shell |
| Date of Submission | 23/07/2026 |


---

## Objectives

- Identify the active shell and explain its role.
- Navigate with `pwd`, `cd`, and `ls`.
- Interpret long listings and reuse command history.

---

## Tools Used

- **`echo`** — displays variables.
- **`pwd`, `ls`, `cd`** — inspect and navigate the filesystem.
- **`history`** — displays previous commands.

---

## Methodology

### 21A. Shell Identification and Listing

```bash
echo $SHELL
pwd
ls
ls -l
```

Confirmed `/bin/zsh`, displayed the current directory, and compared standard and long listings.

### 21B. Navigation and History

```bash
cd Documents
pwd
cd ~
cd ..
history
```

Practised relative navigation, home and parent shortcuts, and reviewed numbered command history.

**Screenshot:** `Fig01_CLI_ShellIdentificationAndListing_EchoShell_Pwd_Ls.png`

<img width="1366" height="662" alt="Fig01_CLI_ShellIdentificationAndListing_EchoShell_Pwd_Ls png" src="https://github.com/user-attachments/assets/d81a6eaa-45e1-4bfe-96de-74705d980adc" />

**Screenshot:** `Fig02_CLI_PathNavigationAndHistory_CdTilde_CdDotDot_History.png`

<img width="1366" height="662" alt="Fig02_CLI_PathNavigationAndHistory_CdTilde_CdDotDot_History png" src="https://github.com/user-attachments/assets/3b9a5d37-434e-4b67-a642-26e96b89f6d9" />


---

## Analysis and Findings

The shell is a stateful interface that tracks the working directory, variables, and history. `ls -l` exposed permissions, ownership, size, and timestamps, all of which are significant in administration and forensic review.

---

## Challenges and Solutions

| Challenge | Solution |
|---|---|
| Relative paths depend on the current location. | Used `pwd` and `ls` before navigation. |
| History can be lengthy. | Used numbered entries to identify relevant commands. |

---

## Conclusion

Lab 21 developed practical competence in using the shell. The exercise connected accurate command use, verification, and safe Linux working practices to administration and cybersecurity workflows.

---

## Screenshots Reference

| Figure | Filename | Lab |
|---|---|---|
| Fig 1 | `Fig01_CLI_ShellIdentificationAndListing_EchoShell_Pwd_Ls.png` | 21 |
| Fig 2 | `Fig02_CLI_PathNavigationAndHistory_CdTilde_CdDotDot_History.png` | 21 |

*Place each screenshot inside `Screenshots/` using the exact filename shown above.*

---

## Recommendations

- Verify location with `pwd` before path-sensitive work.
- Use `ls -l` when metadata is required.
- Review commands before reusing privileged or destructive history entries.

---

## Reference

ICDFA. (2026). *WADF-2026-M02 Week 5 Practical Labs: Labs 21–24, Using the Shell and File Globbing.* International Cybersecurity and Digital Forensics Academy.

