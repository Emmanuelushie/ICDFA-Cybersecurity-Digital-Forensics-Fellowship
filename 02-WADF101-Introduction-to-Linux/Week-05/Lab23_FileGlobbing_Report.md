# Lab 23: File Globbing

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 05 |
| Lab | Lab 23 — File Globbing |
| Date of Submission | 23/07/2026 |


---

## Objectives

- Use `*`, `?`, character classes, negation, and brace expansion.
- Apply patterns to a controlled copy.
- Preview matches before consequential actions.

---

## Tools Used

- **`touch`, `mkdir`** — create test data.
- **`ls`** — previews expansion.
- **`cp`** — copies selected files.

---

## Methodology

### 23A. Test Environment

```bash
cd ~
mkdir globbing_test
cd globbing_test
touch file1.txt file2.txt file3.txt report.txt data.csv notes.md
ls
```

Created six controlled files.

### 23B. Pattern Tests

```bash
ls *.txt
ls file*
ls file?.txt
ls file[12].txt
ls file[^3].txt
ls *.{txt,csv}
```

Tested broad, single-character, character-class, negated-class, and brace-expanded selections.

### 23C. Wildcard Copy

```bash
mkdir backup
cp *.txt backup/
ls backup/
```

Copied all matching text files and verified the destination.

**Screenshot:** `Fig13_CLI_GlobbingPatternTests_Asterisk_QuestionMark_CharacterClasses.png`

<img width="1366" height="662" alt="Fig13_CLI_GlobbingPatternTests_Asterisk_QuestionMark_CharacterClasses png" src="https://github.com/user-attachments/assets/68c05f75-3394-4a91-90f0-c3daaca322ea" />

**Screenshot:** `Fig14_CLI_BraceExpansionAndWildcardCopy_CpTxtBackup.png`

<img width="1366" height="662" alt="Fig14_CLI_BraceExpansionAndWildcardCopy_CpTxtBackup png" src="https://github.com/user-attachments/assets/12b92e03-fe0e-43b3-abf0-de72dfd15eba" />

---

## Analysis and Findings

The shell expands glob patterns before executing the command. `*` matches zero or more characters, `?` matches one, and classes constrain a position. Brace expansion generates alternatives before globbing. Previewing with `ls` reduces unintended selection.

---

## Challenges and Solutions

| Challenge | Solution |
|---|---|
| A broad pattern may match too much. | Previewed every pattern with `ls`. |
| Brace expansion can be confused with globbing. | Recognised that braces generate alternatives before filename matching. |

---

## Conclusion

Lab 23 developed practical competence in file globbing. The exercise connected accurate command use, verification, and safe Linux working practices to administration and cybersecurity workflows.

---

## Screenshots Reference

| Figure | Filename | Lab |
|---|---|---|
| Fig 1 | `Fig13_CLI_GlobbingPatternTests_Asterisk_QuestionMark_CharacterClasses.png` | 23 |
| Fig 2 | `Fig14_CLI_BraceExpansionAndWildcardCopy_CpTxtBackup.png` | 23 |


---

## Recommendations

- Use the narrowest appropriate pattern.
- Preview matches before `cp`, `mv`, or `rm`.
- Test patterns in a dedicated workspace.

---

## Reference

ICDFA. (2026). *WADF-2026-M02 Week 5 Practical Labs: Labs 21–24, Using the Shell and File Globbing.* International Cybersecurity and Digital Forensics Academy.

