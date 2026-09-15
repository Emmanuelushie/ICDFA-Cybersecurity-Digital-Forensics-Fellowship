# Lab 25: Finding Files

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 06 |
| Lab | Lab 25 — Finding Files |
| Date of Submission | 30/07/2026 |

---

## Objectives

- Search for files by name, type, size, modification time, and permissions.
- Combine criteria with implicit AND logic and explicit OR logic.
- Execute another command against matches with `find -exec`.
- Compare live filesystem searching with database-backed `locate`.

## Tools Used

- **`find`** — searches the live filesystem using defined criteria.
- **`locate`** — searches a prebuilt filename database.
- **`updatedb`** — refreshes the database used by `locate`.
- **`wc`** — counts lines in files selected by `find`.
- **`mkdir` and `touch`** — create the controlled test structure.

## Methodology

### 25A. Controlled Search Environment

```bash
mkdir -p ~/find_test/documents ~/find_test/images ~/find_test/archives
cd ~/find_test
touch documents/report.txt documents/notes.txt documents/data.csv
touch images/photo1.jpg images/photo2.jpg images/screenshot.png
touch archives/backup.tar.gz archives/old_files.zip
find ~/find_test -type f
```

Created three subdirectories and eight test files, then verified all regular files before beginning the searches.

**Screenshot:** `Fig01_CLI_FindTestSetup_MkdirTouch_FindTypeF.png`

<img width="1366" height="662" alt="Fig01_CLI_FindTestSetup_MkdirTouch_FindTypeF png" src="https://github.com/user-attachments/assets/c3ab169f-1079-459a-96e3-960d61cb5160" />


### 25B. Name, Type, Size, Time, and Permission Searches

```bash
find ~/find_test -name "*.txt"
find ~/find_test -iname "*.JPG"
```

Searched by case-sensitive and case-insensitive filename patterns.

**Screenshot:** `Fig02_CLI_FindByNameAndCaseInsensitive_FindName_Iname.png`

<img width="1366" height="662" alt="Fig02_CLI_FindByNameAndCaseInsensitive_FindName_Iname png" src="https://github.com/user-attachments/assets/a1f177ae-14a7-4f4c-96cf-23e13757020c" />


```bash
find ~/find_test -type d
find ~/find_test -size +1k
```

Listed directories and searched for objects larger than one kibibyte. Size filtering produces meaningful results only after files contain sufficient data.

**Screenshot:** `Fig03_CLI_FindByTypeAndSize_FindTypeD_FindSize.png`

<img width="1366" height="662" alt="Fig03_CLI_FindByTypeAndSize_FindTypeD_FindSize png" src="https://github.com/user-attachments/assets/4d1dc318-e464-41ce-af3d-acd3fd168725" />


```bash
find ~/find_test -mtime -1
find ~/find_test -perm 644
```

Located recently modified items and files whose permission mode matched `644` exactly.

**Screenshot:** `Fig04_CLI_FindByModTimeAndPermissions_Mtime_Perm.png`

<img width="1366" height="662" alt="Fig04_CLI_FindByModTimeAndPermissions_Mtime_Perm png" src="https://github.com/user-attachments/assets/a890ece2-30c7-495f-9ee3-6713c3f48c8b" />


### 25C. Combined Logic, `-exec`, and `locate`

```bash
find ~/find_test -type f -name "*.txt" -mtime -1
find ~/find_test \( -name "*.txt" -o -name "*.csv" \)
```

Combined regular-file, name, and modification-time conditions with implicit AND logic. Parentheses grouped the OR expression so its scope was explicit.

**Screenshot:** `Fig05_CLI_FindCombinedAndOrLogic_AndTxtMtime_OrCsv.png`

<img width="1366" height="662" alt="Fig05_CLI_FindCombinedAndOrLogic_AndTxtMtime_OrCsv png" src="https://github.com/user-attachments/assets/8fa9ca80-1667-407f-8fe8-85f04f245b19" />

```bash
find ~/find_test -name "*.txt" -exec wc -l {} \;
locate report.txt
sudo updatedb
locate report.txt
```

Passed each text-file match to `wc -l`. The `{}` placeholder represented the current pathname, while `\;` terminated the `-exec` action. The `locate` test demonstrated that newly created files may not appear until the database is refreshed.

**Screenshot:** `Fig06_CLI_FindExecAndLocateUpdatedb_ExecWcL_Locate.png`

<img width="1366" height="662" alt="Fig06_CLI_FindExecAndLocateUpdatedb_ExecWcL_Locate png" src="https://github.com/user-attachments/assets/435cbea1-0e35-4950-ad8c-56e9f4cccd0e" />

---

## Analysis and Findings

`find` searches the current filesystem state and therefore provides current results, while `locate` is faster because it queries an index that may be outdated. The lab also showed that `find` is not only a discovery utility: `-exec` can turn selected paths into inputs for another command. Grouping OR expressions is an important accuracy improvement because ungrouped predicates can produce unexpected logical scope.

---

## Challenges and Solutions

| Challenge | Solution |
|---|---|
| Most files created by `touch` were zero bytes, so `-size +1k` did not initially match them. | Added controlled content where size-based matching was required. |
| A new file did not appear in `locate` results. | Ran `sudo updatedb` and repeated the search. |
| OR conditions can be misinterpreted when ungrouped. | Used escaped parentheses around the OR expression. |

---

## Conclusion

Lab 25 developed practical competence in finding files. The exercise connected accurate command use, verification, and safe Linux working practices to administration, log analysis, and cybersecurity workflows.

---

## Screenshots Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 1 | `Fig01_CLI_FindTestSetup_MkdirTouch_FindTypeF.png` | 25A |
| Fig 2 | `Fig02_CLI_FindByNameAndCaseInsensitive_FindName_Iname.png` | 25B |
| Fig 3 | `Fig03_CLI_FindByTypeAndSize_FindTypeD_FindSize.png` | 25B |
| Fig 4 | `Fig04_CLI_FindByModTimeAndPermissions_Mtime_Perm.png` | 25B |
| Fig 5 | `Fig05_CLI_FindCombinedAndOrLogic_AndTxtMtime_OrCsv.png` | 25C |
| Fig 6 | `Fig06_CLI_FindExecAndLocateUpdatedb_ExecWcL_Locate.png` | 25C |

*Place each screenshot inside `Screenshots/` using the exact filename shown above. The image links in the Methodology section will then resolve automatically.*

---

## Recommendations

- Begin with read-only searches and inspect results before using `-exec`.
- Quote wildcard patterns so the shell does not expand them before `find` receives them.
- Group OR conditions explicitly.
- Treat `locate` results as dependent on database freshness.

---

## Reference

ICDFA. (2026). *WADF-2026-M02 Week 6 Practical Labs: Labs 25–28, Finding Files, Text Utilities, Regular Expressions and the Vi Editor.* International Cybersecurity and Digital Forensics Academy.
