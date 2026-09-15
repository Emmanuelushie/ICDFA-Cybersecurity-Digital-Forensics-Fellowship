# Lab 22: Configuring the Shell

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 05 |
| Lab | Lab 22 — Configuring the Shell |
| Date of Submission | 23/07/2026 |


---

## Objectives

- Inspect `~/.zshrc` and active environment variables.
- Create temporary and persistent aliases.
- Reload and verify shell configuration.

---

## Tools Used

- **`cat`, `env`** — inspect configuration and variables.
- **`alias`** — creates and lists aliases.
- **`nano`, `source`** — edit and reload `~/.zshrc`.

---

## Methodology

### 22A. Configuration Inspection

```bash
cat ~/.zshrc
env
```

Inspected Z shell startup configuration and exported variables such as `PATH`, `HOME`, `USER`, `SHELL`, and `TERM`.

### 22B. Alias Creation

```bash
alias ll='ls -l'
alias h='history'
h
```

Confirmed the existing `ll` alias, created `h`, and tested it.

### 22C. Permanent Configuration

```bash
nano ~/.zshrc
source ~/.zshrc
alias
```

Added `alias h='history'`, reloaded the file, and verified the persistent alias.

**Screenshot:** `Fig03_CLI_ZshrcInspection_CatZshrc.png`

<img width="1366" height="662" alt="Fig03_CLI_ZshrcInspection_CatZshrc png" src="https://github.com/user-attachments/assets/30fa2c4d-e98f-4a6e-9c6d-605db1f0d33b" />

**Screenshot:** `Fig04_CLI_EnvironmentVariables_Env.png`

<img width="1366" height="662" alt="Fig04_CLI_EnvironmentVariables_Env png" src="https://github.com/user-attachments/assets/f1519660-f164-43c0-acdb-739c8c471cc4" />


**Screenshot:** `Fig05_CLI_ExistingAliasLl_AliasLsL.png`

<img width="1366" height="662" alt="Fig05_CLI_ExistingAliasLl_AliasLsL png" src="https://github.com/user-attachments/assets/3a6f7ae3-9907-453f-9b8b-de92e971bba7" />

**Screenshot:** `Fig06_CLI_TemporaryAliasH_AliasHistory.png`

<img width="1366" height="662" alt="Fig06_CLI_TemporaryAliasH_AliasHistory png" src="https://github.com/user-attachments/assets/9098cd8d-e785-4291-a499-dafd12b3bfa9" />


**Screenshot:** `Fig07_CLI_AliasTestRun_H.png`

<img width="1366" height="662" alt="Fig07_CLI_AliasTestRun_H png" src="https://github.com/user-attachments/assets/360767ed-ec54-4df6-b74f-1293d66fe0c2" />


**Screenshot:** `Fig08_CLI_NanoZshrcEdit_AddAliasLine.png`

<img width="1366" height="662" alt="Fig08_CLI_NanoZshrcEdit_AddAliasLine png" src="https://github.com/user-attachments/assets/165e0a04-37aa-486e-bc45-9c89586b0dc3" />


**Screenshot:** `Fig09_CLI_NanoZshrcSaveConfirmation.png`

<img width="1366" height="662" alt="Fig09_CLI_NanoZshrcSaveConfirmation png" src="https://github.com/user-attachments/assets/f9bb8718-cae7-4ecd-8eb7-588e441f56fb" />


**Screenshot:** `Fig10_CLI_SourceZshrcReload.png`

<img width="1366" height="662" alt="Fig10_CLI_SourceZshrcReload png" src="https://github.com/user-attachments/assets/fa95204a-306f-44e1-93f2-d53dcb040731" />


**Screenshot:** `Fig11_CLI_AliasListVerification_Alias.png`

<img width="1366" height="662" alt="Fig11_CLI_AliasListVerification_Alias png" src="https://github.com/user-attachments/assets/f3594a9c-a69e-44a9-89a8-ff21abf00750" />


**Screenshot:** `Fig12_CLI_ShellConfigurationEvidence_Supplementary.png`

<img width="1366" height="662" alt="Fig12_CLI_ShellConfigurationEvidence_Supplementary png" src="https://github.com/user-attachments/assets/da139f5e-5a9a-4afd-8f25-c575d5b26c94" />


---

## Analysis and Findings

A command-line alias lasts for the current session, while an alias in `~/.zshrc` is recreated for new interactive Z shell sessions. Configuration files should be changed carefully because syntax errors can affect every new terminal.

---

## Challenges and Solutions

| Challenge | Solution |
|---|---|
| The `ll` alias already existed. | Preserved it and created a different alias. |
| The new alias was not immediately active. | Reloaded the configuration with `source ~/.zshrc`. |

---

## Conclusion

Lab 22 developed practical competence in configuring the shell. The exercise connected accurate command use, verification, and safe Linux working practices to administration and cybersecurity workflows.

---

## Screenshots Reference

| Figure | Filename | Lab |
|---|---|---|
| Fig 1 | `Fig03_CLI_ZshrcInspection_CatZshrc.png` | 22 |
| Fig 2 | `Fig04_CLI_EnvironmentVariables_Env.png` | 22 |
| Fig 3 | `Fig05_CLI_ExistingAliasLl_AliasLsL.png` | 22 |
| Fig 4 | `Fig06_CLI_TemporaryAliasH_AliasHistory.png` | 22 |
| Fig 5 | `Fig07_CLI_AliasTestRun_H.png` | 22 |
| Fig 6 | `Fig08_CLI_NanoZshrcEdit_AddAliasLine.png` | 22 |
| Fig 7 | `Fig09_CLI_NanoZshrcSaveConfirmation.png` | 22 |
| Fig 8 | `Fig10_CLI_SourceZshrcReload.png` | 22 |
| Fig 9 | `Fig11_CLI_AliasListVerification_Alias.png` | 22 |
| Fig 10 | `Fig12_CLI_ShellConfigurationEvidence_Supplementary.png` | 22 |

---

## Recommendations

- Back up `~/.zshrc` before major edits.
- Avoid storing secrets in shell configuration.
- Test changes in the current session before opening a new terminal.

---

## Reference

ICDFA. (2026). *WADF-2026-M02 Week 5 Practical Labs: Labs 21–24, Using the Shell and File Globbing.* International Cybersecurity and Digital Forensics Academy.

