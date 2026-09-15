# Week 06: Finding Files, Text Utilities, Regular Expressions and the Vi Editor

## 👤 Author

**Emmanuel Adie Ushie**  
**WADF-2026-M02: Introduction to Linux**  
**International Cybersecurity and Digital Forensics Academy**

## 📘 Overview

This folder contains the practical laboratory work completed during Week 06 of the Introduction to Linux module at the International Cybersecurity and Digital Forensics Academy (ICDFA). The week progressed from locating filesystem objects to extracting and transforming text, matching structured patterns with regular expressions, and editing files directly from the terminal with Vi.

The documentation is structured for academic assessment and professional GitHub portfolio presentation. Each report contains reproducible commands, explanations, findings, safety considerations, troubleshooting notes, and screenshot references.

## 📘 Week 06 Learning Areas

### Lab 25: Finding Files

Topics covered:
- Building a controlled search workspace
- Searching by filename and case-insensitive name
- Filtering by file type, size, modification time, and permissions
- Combining criteria with AND and OR logic
- Acting on results with `find -exec`
- Comparing live `find` searches with database-backed `locate`

📑 **Lab Report:** `Lab25_FindingFiles_Report.md`

### Lab 26: Text Utilities

Topics covered:
- Creating a controlled CSV-style dataset
- Extracting fields with `cut`
- Alphabetical and numeric sorting
- Removing and counting duplicates with `uniq`
- Translating and deleting characters with `tr`
- Counting lines, words, and characters with `wc`
- Building multi-stage pipelines

📑 **Lab Report:** `Lab26_TextUtilities_Report.md`

### Lab 27: Regular Expressions

Topics covered:
- Literal matches and the dot metacharacter
- Asterisk, plus, and exact-count quantifiers
- Character and negated character classes
- Start and end anchors
- Email and phone-number pattern matching
- Case-insensitive searches

📑 **Lab Report:** `Lab27_RegularExpressions_Report.md`

### Lab 28: The Vi Editor

Topics covered:
- Opening and closing files in Vi
- Command mode and insert mode
- Keyboard navigation
- Deleting, undoing, and redoing changes
- Searching within a file
- Global search and replacement
- Saving, exiting, and abandoning edits safely

📑 **Lab Report:** `Lab28_ViEditor_Report.md`

## 🧪 Tools and Environment

- **Kali Linux** — Linux laboratory operating system
- **VirtualBox** — virtualisation platform
- **Z shell (`zsh`)** — active command-line interpreter
- **`find`, `locate`, `updatedb`** — filesystem search utilities
- **`cut`, `sort`, `uniq`, `tr`, `wc`** — text-processing utilities
- **`grep` and extended regular expressions** — pattern matching
- **Vi** — terminal-based modal text editor
- **`mkdir`, `touch`, `cat`, heredocs** — controlled test-data creation
- **GitHub** — documentation and portfolio platform

## 📁 Evidence Structure

```text
Week06_LinuxEssentials/
├── README.md
├── Lab25_FindingFiles_Report.md
├── Lab26_TextUtilities_Report.md
├── Lab27_RegularExpressions_Report.md
├── Lab28_ViEditor_Report.md
└── Screenshots/
    ├── Fig01_CLI_FindTestSetup_MkdirTouch_FindTypeF.png
    ├── Fig02_CLI_FindByNameAndCaseInsensitive_FindName_Iname.png
    ├── Fig03_CLI_FindByTypeAndSize_FindTypeD_FindSize.png
    ├── Fig04_CLI_FindByModTimeAndPermissions_Mtime_Perm.png
    ├── Fig05_CLI_FindCombinedAndOrLogic_AndTxtMtime_OrCsv.png
    ├── Fig06_CLI_FindExecAndLocateUpdatedb_ExecWcL_Locate.png
    ├── Fig07_CLI_CsvDatasetCreationAndCut_HeredocCut.png
    ├── Fig08_CLI_SortDefaultAndNumericByField_Sort_SortK3n.png
    ├── Fig09_CLI_UniqDedupeAndCount_SortUniq_UniqC.png
    ├── Fig10_CLI_TrTranslateDeleteAndWcCounts_TrUpper_TrD_Wc.png
    ├── Fig11_CLI_RegexTestFileAndLiteralDotMatch_GrepFox_DotG.png
    ├── Fig12_CLI_RegexQuantifiersAndCharacterClasses_StarPlus_Digits_Negated.png
    ├── Fig13_CLI_RegexAnchorsEmailPhonePattern_CaretDollar_ExtendedRegex.png
    ├── Fig14_CLI_ViTestFileAndOpen_CatHeredoc_ViOpen.png
    ├── Fig15_CLI_ViInsertAndDeleteCommands_I_Dw_Dd_U_CtrlR.png
    ├── Fig16_CLI_ViSearchAndGlobalReplace_SlashSearch_PercentSg.png
    └── Fig17_CLI_ViSaveAndExit_WWqQBang.png
```

## 📸 Screenshot Evidence

| Figures | Evidence | Lab |
|---|---|---|
| Figs 1–6 | Search setup, filters, logic, `-exec`, and `locate` | Lab 25 |
| Figs 7–10 | Dataset creation, extraction, sorting, deduplication, transformation, and counts | Lab 26 |
| Figs 11–13 | Literal matches, quantifiers, classes, anchors, email, and phone patterns | Lab 27 |
| Figs 14–17 | Vi opening, editing, searching, replacing, saving, and exiting | Lab 28 |

**Evidence standard:** Screenshots should clearly display the relevant command and output. Unrelated applications and unnecessary desktop content should be excluded. Screenshot names must match the report references exactly.

## 🎯 Learning Outcomes

By completing Week 06, I developed practical ability to:
- Locate files with precise filesystem-search criteria.
- Distinguish live filesystem searches from database-backed searches.
- Apply commands directly to search results with `find -exec`.
- Extract, sort, deduplicate, transform, and count text data.
- Combine single-purpose utilities into efficient pipelines.
- Build and interpret regular expressions for structured data.
- Use anchors, classes, quantifiers, and case-insensitive matching accurately.
- Navigate and edit files in Vi without a graphical interface.
- Search, replace, undo, save, exit, and discard changes safely.
- Document practical Linux work with reproducible evidence.

## 🔐 Evidence and Safety

All exercises were performed in a controlled Kali Linux virtual machine using purpose-built files and directories. Search actions were initially read-only. Text processing preserved the source datasets. Vi editing was practised on a disposable test file. No unauthorised system or third-party infrastructure was targeted.

## 📌 Disclaimer

This repository is intended for educational and academic purposes only. All activities were performed in a controlled laboratory environment using test data.

## 📚 Reference

ICDFA. (2026). *WADF-2026-M02 Week 6 Practical Labs: Labs 25–28, Finding Files, Text Utilities, Regular Expressions and the Vi Editor.* International Cybersecurity and Digital Forensics Academy.

