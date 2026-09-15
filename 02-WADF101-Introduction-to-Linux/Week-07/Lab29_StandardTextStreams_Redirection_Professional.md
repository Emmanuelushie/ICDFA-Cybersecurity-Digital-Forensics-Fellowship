# Lab 29: Standard Text Streams and Redirection

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 07 |
| Lab | Lab 29: Standard Text Streams and Redirection |
| Date of Submission | 06/08/2026 |


---

## Executive Summary

This report documents Lab 29 of Week 07. The lab examined the three standard Linux streams and applied shell operators to redirect input, preserve or replace output, capture errors, combine streams, discard unwanted messages, construct pipelines, and save visible output with `tee`.

The exercises showed that standard output and standard error are independent channels. Accurate command logging therefore requires deliberate handling of both streams. Multi-stage pipelines also demonstrated how small Linux utilities can be combined to transform data without temporary intermediate files.

---

## Objectives

- Identify standard input (`0`), standard output (`1`), and standard error (`2`).
- Use `>`, `>>`, and `<` correctly.
- Capture errors with `2>` and combine streams with `2>&1`.
- Discard selected output through `/dev/null`.
- Build pipelines with `|`.
- Use `tee` to display and save output simultaneously.

---

## Tools and Environment

- **Kali Linux or Ubuntu virtual machine:** Controlled laboratory system.
- **Bash:** Interpreted redirection and pipeline operators.
- **`echo` and `cat`:** Produced and displayed text.
- **`sort` and `uniq`:** Ordered data and counted or removed repeated values.
- **`tee`:** Duplicated output to the terminal and a file.
- **`/dev/null`:** Discarded selected output.

---

## Workspace and Evidence Storage

```text
/media/sf_ICDFA/Week07/
/media/sf_ICDFA/Week07/Screenshots/
/media/sf_ICDFA/Week07/Notes/
```

---

## Methodology

### 29A. Standard Output, Append, and Input Redirection

```bash
echo "This goes to stdout"
echo "Hello World" > ~/output.txt
cat ~/output.txt
echo "Second line" >> ~/output.txt
cat ~/output.txt
sort < ~/output.txt
```

The first command confirmed that normal command output is written to standard output. The single `>` operator created or replaced `output.txt`, while `>>` appended a second line without removing the first. The `<` operator supplied the file to `sort` as standard input.

**Screenshot Filename:** `Fig01_CLI_StdoutRedirectionAndAppend_EchoRedirect_AppendRedirect.png`

<img width="1366" height="662" alt="Fig01_CLI_StdoutRedirectionAndAppend_EchoRedirect_AppendRedirect png" src="https://github.com/user-attachments/assets/2edef0de-61e1-4aa6-8398-56d9f6151b45" />


---

### 29B. Error Redirection and Combined Streams

```bash
ls /nonexistent 2> ~/error.txt
cat ~/error.txt
ls /nonexistent > ~/output.txt 2>&1
ls /nonexistent 2> /dev/null
```

`2>` captured only standard error. In the combined command, standard output was redirected first and `2>&1` then directed standard error to the destination currently used by standard output. Redirecting errors to `/dev/null` suppressed their display.

> Redirection order matters. `command > file 2>&1` combines both streams in `file`, while `command 2>&1 > file` may leave standard error connected to the terminal.

### 29C. Pipelines and `tee`

```bash
cat ~/output.txt | sort | uniq
echo -e "apple
banana
apple
cherry" | sort | uniq -c | sort -rn
echo "Important data" | tee ~/important.txt
cat ~/important.txt
```

The first pipeline ordered the input and removed adjacent duplicates. The second grouped values, counted occurrences, and ranked the counts numerically in descending order. `tee` displayed the text and saved it to `important.txt` simultaneously.

**Screenshot Filename:** `Fig02_CLI_StderrCaptureCombinedStreamsAndPipelines_2Redirect_Tee.png`

<img width="1366" height="662" alt="Fig02_CLI_StderrCaptureCombinedStreamsAndPipelines_2Redirect_Tee png" src="https://github.com/user-attachments/assets/b9c6f00e-67d6-463a-beaf-ff2d19ebcabf" />

---

## Results and Findings

The lab confirmed that a command can produce normal output and errors through separate descriptors. Capturing only standard output can therefore create an incomplete record. `2>&1` is useful when a single chronological log is required, whereas separate files may be preferable when errors require independent review.

Pipelines supported efficient transformation without manually saving each intermediate result. The placement of `sort` before `uniq` was essential because `uniq` processes adjacent repeated lines rather than searching globally for duplicates.

---

## Challenges and Solutions

| Challenge | Cause | Resolution |
|---|---|---|
| Earlier captured content was overwritten. | A later command used `>` instead of `>>`. | The command was rerun with `>>`, and the resulting file was verified with `cat`. |

---

## Screenshot Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 1 | `Fig01_CLI_StdoutRedirectionAndAppend_EchoRedirect_AppendRedirect.png` | 29A |
| Fig 2 | `Fig02_CLI_StderrCaptureCombinedStreamsAndPipelines_2Redirect_Tee.png` | 29B–29C |

Place each screenshot in `Screenshots/` using the exact filename shown above.

---

## Recommendations

- Choose `>` only when replacement is intentional and `>>` when preserving existing content.
- Capture standard error explicitly when producing technical evidence.
- Verify redirected files with a read-only command such as `cat` or `less`.
- Keep redirection order explicit when combining streams.
- Sort data before using `uniq` for complete grouping.

---

## Conclusion

Lab 29 established practical control over Linux data flow. Standard streams, redirection, pipelines, and `tee` were used to create complete and reproducible command outputs. These skills are essential for scripting, troubleshooting, evidence collection, and log processing.

---

## Safety and Evidence Handling

All commands used controlled test files. Source information was preserved where required, and outputs were written only to authorised laboratory paths.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 7 Practical Labs: Labs 29–32, Standard Text Streams, Processes, Archives and File Permissions.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
3. GNU Project. (n.d.). *GNU tar Manual.* https://www.gnu.org/software/tar/manual/
