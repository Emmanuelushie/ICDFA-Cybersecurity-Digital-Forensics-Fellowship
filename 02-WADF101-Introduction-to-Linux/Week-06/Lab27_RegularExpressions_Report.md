# Lab 27: Regular Expressions

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 06 |
| Lab | Lab 27 — Regular Expressions |
| Date of Submission | 30/07/2026 |

---

## Objectives

- Apply literal, wildcard, quantified, class-based, and anchored regular expressions.
- Distinguish basic and extended regular-expression syntax.
- Match email-address and telephone-number structures.
- Perform case-insensitive searches.

## Tools Used

- **`grep`** — searches text for matching lines.
- **`grep -E`** — enables extended regular expressions.
- **`grep -i`** — performs case-insensitive matching.
- **Character classes, anchors, and quantifiers** — define structured patterns.

## Methodology

### 27A. Controlled Regex Dataset

```bash
cat > ~/regex_test.txt <<'EOF'
The quick brown fox jumps over the lazy dog
Email: john@example.com
Phone: 555-1234
Date: 2026-07-18
Price: $99.99
Username: user123
Password: Pass@Word123
EOF
```

Created a seven-line file containing prose and controlled structured values.

### 27B. Literal, Dot, and Quantifier Matching

```bash
grep "fox" ~/regex_test.txt
grep "d.g" ~/regex_test.txt
grep "fo*x" ~/regex_test.txt
grep -E "fo+x" ~/regex_test.txt
```

Compared a literal match, the dot metacharacter, zero-or-more repetition, and one-or-more repetition.

**Screenshot:** `Fig11_CLI_RegexTestFileAndLiteralDotMatch_GrepFox_DotG.png`

<img width="1366" height="662" alt="Fig11_CLI_RegexTestFileAndLiteralDotMatch_GrepFox_DotG png" src="https://github.com/user-attachments/assets/d2d80ea9-4241-4d13-97af-61d4e6242837" />


### 27C. Character Classes and Anchors

```bash
grep "[0-9]" ~/regex_test.txt
grep "[^0-9]" ~/regex_test.txt
grep "^Email" ~/regex_test.txt
grep "dog$" ~/regex_test.txt
```

Matched digits, non-digits, text at the beginning of a line, and text at the end of a line. A caret inside brackets negates a class, while a caret outside brackets anchors the start.

**Screenshot:** `Fig12_CLI_RegexQuantifiersAndCharacterClasses_StarPlus_Digits_Negated.png`

<img width="1366" height="662" alt="Fig12_CLI_RegexQuantifiersAndCharacterClasses_StarPlus_Digits_Negated png" src="https://github.com/user-attachments/assets/b1ba8135-2a8f-4559-b947-138f1a364e47" />


### 27D. Structured and Case-Insensitive Patterns

```bash
grep -E '[a-zA-Z0-9]+@[a-zA-Z0-9]+\.[a-zA-Z]+' ~/regex_test.txt
grep -E '[0-9]{3}-[0-9]{4}' ~/regex_test.txt
grep -i 'password' ~/regex_test.txt
```

Matched the controlled email structure, matched exactly three digits followed by a hyphen and four digits, and located the password label without case sensitivity.

**Screenshot:** `Fig13_CLI_RegexAnchorsEmailPhonePattern_CaretDollar_ExtendedRegex.png`

<img width="1366" height="662" alt="Fig13_CLI_RegexAnchorsEmailPhonePattern_CaretDollar_ExtendedRegex png" src="https://github.com/user-attachments/assets/279802dd-344e-4e45-82d5-56de243de8cc" />


---

## Analysis and Findings

Regular expressions extended `grep` from literal keyword searching to structural matching. Position changes meaning: `^` anchors the start outside a class but negates a class inside brackets. Extended mode made `+` and `{n}` convenient for structured values. The email expression was suitable for the controlled lab data but should not be treated as a complete validator for every valid email address.

---

## Challenges and Solutions

| Challenge | Solution |
|---|---|
| The caret has different meanings in different positions. | Compared `^Email` with `[^0-9]` and documented the distinction. |
| Basic and extended regex quantifiers differ. | Used `grep -E` for `+` and `{n}`. |
| A dot normally matches any character. | Escaped the dot when a literal period was required. |

---

## Conclusion

Lab 27 developed practical competence in regular expressions. The exercise connected accurate command use, verification, and safe Linux working practices to administration, log analysis, and cybersecurity workflows.

---

## Screenshots Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 11 | `Fig11_CLI_RegexTestFileAndLiteralDotMatch_GrepFox_DotG.png` | 27A–27B |
| Fig 12 | `Fig12_CLI_RegexQuantifiersAndCharacterClasses_StarPlus_Digits_Negated.png` | 27C |
| Fig 13 | `Fig13_CLI_RegexAnchorsEmailPhonePattern_CaretDollar_ExtendedRegex.png` | 27D |

*Place each screenshot inside `Screenshots/` using the exact filename shown above. The image links in the Methodology section will then resolve automatically.*

---

## Recommendations

- Quote regex patterns to reduce shell interpretation.
- Use anchors when the position of a match matters.
- Test expressions against expected matches and non-matches.
- Treat simplified lab patterns as search patterns, not universal data validators.

---

## Reference

ICDFA. (2026). *WADF-2026-M02 Week 6 Practical Labs: Labs 25–28, Finding Files, Text Utilities, Regular Expressions and the Vi Editor.* International Cybersecurity and Digital Forensics Academy.
