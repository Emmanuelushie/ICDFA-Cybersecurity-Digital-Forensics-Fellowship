# Lab 28: The Vi Editor

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 06 |
| Lab | Lab 28 — The Vi Editor |
| Date of Submission | 30/07/2026 |

---

## Objectives

- Open and close files in Vi.
- Distinguish command mode from insert mode.
- Navigate with character, word, line, and file motions.
- Delete, undo, and redo edits.
- Search and replace text.
- Save, exit, or discard changes deliberately.

## Tools Used

- **Vi** — modal terminal text editor.
- **`h`, `j`, `k`, `l`, `w`, `b`, `0`, `$`, `G`, `1G`** — navigation commands.
- **`i`, `Esc`** — enter insert mode and return to command mode.
- **`dw`, `dd`, `d$`, `u`, `Ctrl+r`** — edit, undo, and redo commands.
- **`/`, `n`, `N`, `:%s`** — search and substitution commands.

## Methodology

### 28A. Test File and Editor Modes

```bash
cat > ~/vi_test.txt <<'EOF'
This is line one
This is line two
This is line three
This is line four
This is line five
EOF

vi ~/vi_test.txt
```

Created a disposable five-line file and opened it in Vi. Vi started in command mode. The `i` key entered insert mode, while `Esc` returned to command mode.

**Screenshot:** `Fig14_CLI_ViTestFileAndOpen_CatHeredoc_ViOpen.png`

<img width="1366" height="662" alt="Fig14_CLI_ViTestFileAndOpen_CatHeredoc_ViOpen png" src="https://github.com/user-attachments/assets/3189abc3-1255-4619-b4c8-e5128b4919f9" />

### 28B. Navigation and Editing

```text
h  j  k  l
w  b
0  $
G  1G
i  Esc
dw  dd  d$
u  Ctrl+r
```

Practised character, word, line, and file navigation. Inserted text, deleted a word, a line, and the remainder of a line, then tested undo and redo.

**Screenshot:** `Fig15_CLI_ViInsertAndDeleteCommands_I_Dw_Dd_U_CtrlR.png`

<img width="1366" height="662" alt="Fig15_CLI_ViInsertAndDeleteCommands_I_Dw_Dd_U_CtrlR png" src="https://github.com/user-attachments/assets/ded218cf-95bf-4a89-b567-630fcc02c3c2" />

### 28C. Search and Global Replacement

```text
/fox
n
N
:%s/line/LINE/g
```

Searched forward for a term, moved between matches, and replaced every occurrence of `line` with `LINE` throughout the file. The `%` selected all lines and the trailing `g` replaced every match on each selected line.

**Screenshot:** `Fig16_CLI_ViSearchAndGlobalReplace_SlashSearch_PercentSg.png`

<img width="1366" height="662" alt="Fig16_CLI_ViSearchAndGlobalReplace_SlashSearch_PercentSg png" src="https://github.com/user-attachments/assets/7833c015-7cbc-4050-ae82-0fc6a8436bd8" />


### 28D. Saving and Exiting

```text
:w
:wq
:q!
```

Used `:w` to save without exiting, `:wq` to save and exit, and `:q!` to abandon unsaved changes deliberately.

**Screenshot:** `Fig17_CLI_ViSaveAndExit_WWqQBang.png`

<img width="1366" height="662" alt="Fig17_CLI_ViSaveAndExit_WWqQBang png" src="https://github.com/user-attachments/assets/a808ece0-005e-4111-87e7-a73b9c536e45" />


---

## Analysis and Findings

Vi requires awareness of the active mode because the same key has different effects in command and insert modes. Keyboard motions support efficient editing without a mouse. Undo, redo, search, global substitution, and explicit save or quit commands provide a complete terminal editing workflow suitable for local and remote Linux systems.

---

## Challenges and Solutions

| Challenge | Solution |
|---|---|
| Typing in command mode triggered commands instead of inserting text. | Entered insert mode with `i` and returned with `Esc` before commands. |
| A deletion removed more text than intended. | Used `u` to undo and selected a more precise deletion command. |
| Unsaved experimental edits needed to be abandoned. | Used `:q!` only when discarding changes was intentional. |

---

## Conclusion

Lab 28 developed practical competence in the vi editor. The exercise connected accurate command use, verification, and safe Linux working practices to administration, log analysis, and cybersecurity workflows.

---

## Screenshots Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 14 | `Fig14_CLI_ViTestFileAndOpen_CatHeredoc_ViOpen.png` | 28A |
| Fig 15 | `Fig15_CLI_ViInsertAndDeleteCommands_I_Dw_Dd_U_CtrlR.png` | 28B |
| Fig 16 | `Fig16_CLI_ViSearchAndGlobalReplace_SlashSearch_PercentSg.png` | 28C |
| Fig 17 | `Fig17_CLI_ViSaveAndExit_WWqQBang.png` | 28D |

*Place each screenshot inside `Screenshots/` using the exact filename shown above. The image links in the Methodology section will then resolve automatically.*

---

## Recommendations

- Practise on disposable files before editing system configuration.
- Press `Esc` before entering command-line instructions beginning with `:`.
- Save periodically with `:w`.
- Use `:q!` only when unsaved changes should definitely be discarded.

---

## Reference

ICDFA. (2026). *WADF-2026-M02 Week 6 Practical Labs: Labs 25–28, Finding Files, Text Utilities, Regular Expressions and the Vi Editor.* International Cybersecurity and Digital Forensics Academy.
