# Week 02: Linux Essentials (WADF-2026-M01)

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | Linux Essentials — WADF-2026-M01 |
| Instructor | Mr. Udam Akume Gabriel |
| Institution | International Cybersecurity and Digital Forensics Academy (ICDFA) |
| Week | Week 02 |
| Labs Covered | Lab 3 — File Management & Discovery; Lab 4 — Archiving, Compression & Text Processing |

---

## Overview

This folder contains the Week 02 deliverables for the Linux Essentials module. The two labs this week moved from basic navigation into practical, hands-on Linux administration: building an organised project workspace, managing the full lifecycle of files, locating files with wildcards and `find`, packaging and compressing data into verifiable archives, and processing a log file with core text-processing utilities.

These are foundational skills for digital forensics work — the same techniques used here (structured evidence storage, safe file handling, archive integrity verification via checksums, and log analysis) map directly onto how evidence is collected, packaged, and validated in real investigations.

---

## Contents

| File | Description |
|---|---|
| `README.md` | This overview file |
| `Lab3_FileManagementAndDiscovery.md` | Full report for Lab 3 — building the project workspace, file lifecycle management, wildcards vs. `find` |
| `Lab4_ArchivingCompressionTextProcessing.md` | Full report for Lab 4 — tar/gzip archiving, SHA-256 integrity checks, restore testing, text-utility log analysis |
| `Screenshots/` | Terminal screenshot evidence, referenced by figure number in each lab report |

---

## Tools and Environment

- **Kali Linux (Virtual Machine)** — primary environment for all commands and file operations
- **Bash shell** — command-line interface used throughout
- **`tar` / `gzip`** — archive creation and compression
- **`sha256sum`** — archive integrity verification
- **`find`, wildcards (`*`)** — file discovery
- **`head`, `tail`, `grep`, `cut`, `sort`, `uniq`, `wc`** — text processing and log analysis
- **`diff`** — file comparison

---

## Key Takeaways

- Organised, predictable folder structures (built with `mkdir -p` and brace expansion) make project work easier to manage and audit.
- The full file lifecycle, create, copy, edit, compare, move, safely delete can be handled entirely and safely from the command line.
- `find` is a deep search tool (name, size, date, type across a whole tree), while wildcards are a fast shortcut limited to a single folder.
- SHA-256 checksums provide a tamper-evident fingerprint for archives, which is foundational to evidence integrity in forensics work.
- Chaining simple text utilities (`cut | sort | uniq -c`) is a fast, scriptable way to summarise large log files without manual review.

See the individual lab reports for full methodology, command-by-command breakdowns, findings, and screenshots.

