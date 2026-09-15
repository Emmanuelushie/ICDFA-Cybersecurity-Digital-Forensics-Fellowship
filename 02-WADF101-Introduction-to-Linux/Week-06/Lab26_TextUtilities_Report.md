# Lab 26: Text Utilities

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 06 |
| Lab | Lab 26 — Text Utilities |
| Date of Submission | 30/07/2026 |

---

## Objectives

- Extract selected fields from delimited text with `cut`.
- Sort complete lines and numeric fields.
- Remove and count duplicate records with `uniq`.
- Translate and delete characters with `tr`.
- Count lines, words, and characters with `wc`.
- Combine utilities in pipelines.

## Tools Used

- **`cut`** — extracts delimited fields.
- **`sort`** — orders lines alphabetically or numerically by key.
- **`uniq`** — removes or counts consecutive duplicate lines.
- **`tr`** — translates or deletes characters from standard input.
- **`wc`** — counts lines, words, bytes, or characters.

## Methodology

### 26A. Dataset Creation and Field Extraction

```bash
cat > ~/textutils_data.txt <<'EOF'
John,Engineering,75000
Sarah,Marketing,65000
Mike,Engineering,72000
Lisa,Sales,60000
John,Engineering,75000
EOF

cut -d',' -f1 ~/textutils_data.txt
cut -d',' -f1,3 ~/textutils_data.txt
```

Created a five-record CSV-style dataset containing one duplicate, then extracted the name field and the non-adjacent name and salary fields.

**Screenshot:** `Fig07_CLI_CsvDatasetCreationAndCut_HeredocCut.png`

<img width="1366" height="662" alt="Fig07_CLI_CsvDatasetCreationAndCut_HeredocCut png" src="https://github.com/user-attachments/assets/380a8a57-e3f7-447c-809b-357bb2f066db" />



### 26B. Sorting and Duplicate Analysis

```bash
sort ~/textutils_data.txt
sort -t',' -k3,3n ~/textutils_data.txt
sort -t',' -k3,3nr ~/textutils_data.txt
```

Sorted full records alphabetically, then sorted the third comma-delimited field numerically in ascending and descending order.

**Screenshot:** `Fig08_CLI_SortDefaultAndNumericByField_Sort_SortK3n.png`

<img width="1366" height="662" alt="Fig08_CLI_SortDefaultAndNumericByField_Sort_SortK3n png" src="https://github.com/user-attachments/assets/399e85f2-a908-4df3-b52e-eda2e283952c" />


```bash
sort ~/textutils_data.txt | uniq
sort ~/textutils_data.txt | uniq -c
```

Placed identical records next to one another before collapsing and counting them. `uniq` only compares adjacent lines.

**Screenshot:** `Fig09_CLI_UniqDedupeAndCount_SortUniq_UniqC.png`

<img width="1366" height="662" alt="Fig09_CLI_UniqDedupeAndCount_SortUniq_UniqC png" src="https://github.com/user-attachments/assets/5b6485e4-5a4e-43b2-a004-32e8639d5f50" />

### 26C. Translation, Deletion, and Counting

```bash
tr 'a-z' 'A-Z' < ~/textutils_data.txt
tr -d ',' < ~/textutils_data.txt
wc ~/textutils_data.txt
wc -l ~/textutils_data.txt
```

Converted lowercase letters to uppercase, removed comma characters from streamed output, and counted all metrics and lines. The source file remained unchanged because no output was redirected back to it.

**Screenshot:** `Fig10_CLI_TrTranslateDeleteAndWcCounts_TrUpper_TrD_Wc.png`

<img width="1366" height="662" alt="Fig10_CLI_TrTranslateDeleteAndWcCounts_TrUpper_TrD_Wc png" src="https://github.com/user-attachments/assets/ff43d751-de82-4fc9-a568-717db073004c" />


---

## Analysis and Findings

The lab demonstrated the Unix principle of composing small utilities. `cut` selected fields, `sort` established the order needed by `uniq`, `uniq -c` exposed repeated records, `tr` transformed streams, and `wc` measured data volume. The dependency between `sort` and `uniq` was especially important because duplicates that are not consecutive are not collapsed.

---

## Challenges and Solutions

| Challenge | Solution |
|---|---|
| The duplicate records were separated in the source data. | Sorted the complete records before running `uniq`. |
| Salary values required numeric rather than lexical sorting. | Used a comma delimiter, the third field as the key, and the numeric option. |
| `tr` does not accept a filename as its normal data argument. | Redirected the file into standard input with `<`. |

---

## Conclusion

Lab 26 developed practical competence in text utilities. The exercise connected accurate command use, verification, and safe Linux working practices to administration, log analysis, and cybersecurity workflows.

---

## Screenshots Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 7 | `Fig07_CLI_CsvDatasetCreationAndCut_HeredocCut.png` | 26A |
| Fig 8 | `Fig08_CLI_SortDefaultAndNumericByField_Sort_SortK3n.png` | 26B |
| Fig 9 | `Fig09_CLI_UniqDedupeAndCount_SortUniq_UniqC.png` | 26B |
| Fig 10 | `Fig10_CLI_TrTranslateDeleteAndWcCounts_TrUpper_TrD_Wc.png` | 26C |

*Place each screenshot inside `Screenshots/` using the exact filename shown above. The image links in the Methodology section will then resolve automatically.*

---

## Recommendations

- Preserve source data and direct transformed output to a new file when required.
- Sort on the correct key before using `uniq`.
- Use numeric sorting for numeric values.
- Verify delimiters and field positions before extracting columns.

---

## Reference

ICDFA. (2026). *WADF-2026-M02 Week 6 Practical Labs: Labs 25–28, Finding Files, Text Utilities, Regular Expressions and the Vi Editor.* International Cybersecurity and Digital Forensics Academy.
