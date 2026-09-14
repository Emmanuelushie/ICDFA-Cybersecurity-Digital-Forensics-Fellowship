## 👤 Author

Emmanuel Adie Ushie  
Linux Essentials: WADF-2026-M01  
International Cybersecurity and Digital Forensics Academy

## Week 02: Managing Files, Archiving, Compression and Text Processing

This folder contains the hands-on laboratory work completed during Week 02 of
the Linux Essentials module at the International Cybersecurity and Digital
Forensics Academy (ICDFA).  
The week focused on building and maintaining a controlled project workspace,
managing files through their full lifecycle, discovering files with wildcards
and the `find` command, creating and validating archives, applying compression,
verifying file integrity with SHA-256 checksums, and processing simulated log
data with standard Linux text utilities.  
The documentation is structured for both academic submission and
professional GitHub portfolio presentation, with command evidence,
explanations, findings, and dedicated spaces for screenshot verification.

## 📘 Week 02 Learning Areas

### Lab 3: File Management & Discovery

This lab covers structured workspace creation, safe file management, and file
discovery techniques.  
Topics covered:
- Creating nested project directories with `mkdir -p`
- Using brace expansion to build folder structures
- Creating and documenting files with `touch` and `printf`
- Copying, editing, comparing, moving, and safely deleting files
- Appending content without overwriting existing data
- Comparing file versions with `diff`
- Using wildcards for quick filename matching
- Finding files by name, size, type, and modification date
- Understanding wildcard expansion versus recursive `find` searches  
📑 Lab Report:
Lab03\_FileManagement\_Discovery\_Report.md

### Lab 4: Archiving, Compression & Text Processing

This lab focuses on packaging files, validating archive integrity, testing
restoration, and processing text data from the Linux command line.  
Topics covered:
- Creating and inspecting tar archives
- Applying gzip compression to archives
- Comparing compressed and uncompressed archive sizes
- Generating SHA-256 integrity checksums
- Extracting archives into a controlled restore-test directory
- Verifying restored archive contents
- Reading log data with `head` and `tail`
- Searching and counting text with `grep` and `wc`
- Building pipelines with `cut`, `sort`, and `uniq`
- Safe output redirection with `>` and `>>`  
📑 Lab Report:
Lab04\_Archiving\_Compression\_TextProcessing\_Report.md

## 🧪 Tools and Environment
- Kali Linux — Linux laboratory operating system
- VirtualBox — virtualisation platform used to run the Linux VM
- Bash Shell — command-line environment used for the practical exercises
- tar and gzip — archive creation and compression utilities
- sha256sum — archive integrity verification utility
- grep, cut, sort, uniq, wc, head, and tail — text processing utilities
- diff — file comparison utility
- find — recursive file discovery utility
- VirtualBox Shared Folder and Python HTTP Server — laboratory file transfer methods
- GitHub — documentation and academic submission platform

## 📁 Evidence Structure

Week02\_LinuxEssentials/
├── README.md
├── Lab03\_FileManagement\_Discovery\_Report.md
├── Lab04\_Archiving\_Compression\_TextProcessing\_Report.md
└── Screenshots/
├── Fig01\_CLI\_ProjectWorkspaceSetup\_MkdirP\_PrintfReadme.png
├── Fig02\_CLI\_ProjectStructureVerification\_Find\_Cat.png
├── Fig03\_CLI\_FileLifecycle\_TouchCopyEdit\_Diff.png
├── Fig04\_CLI\_FileMoveAndSafeDelete\_Mv\_RmI.png
├── Fig05\_CLI\_WildcardVsFind\_EchoGlob\_FindName.png
├── Fig06\_CLI\_FindBySizeAndDate\_FindSize\_Mtime.png
├── Fig07\_CLI\_TarArchiveCreation\_TarCvf\_TarTf\_TarCzvf.png
├── Fig08\_CLI\_ArchiveIntegrityAndRestore\_Sha256sum\_TarXzvf.png
├── Fig09\_CLI\_LogFileTextUtilities\_Head\_Tail\_Wc\_Grep.png
└── Fig10\_CLI\_LogSummaryPipelineAndRedirection\_CutSortUniq\_DateAppend.png

## 📸 Screenshot Evidence

Screenshots are placed in the Screenshots/ directory and embedded in the
relevant lab reports immediately after the commands or activity they verify.
<table>
<tr>
<th>  
Figure
</th>
<th>  
Evidence
</th>
<th>  
Report Section
</th>
</tr>
<tr>
<td>  
Fig 1
</td>
<td>  
Project workspace setup
</td>
<td>  
Lab 3 — 3A
</td>
</tr>
<tr>
<td>  
Fig 2
</td>
<td>  
Project structure verification
</td>
<td>  
Lab 3 — 3A
</td>
</tr>
<tr>
<td>  
Fig 3
</td>
<td>  
File lifecycle and comparison
</td>
<td>  
Lab 3 — 3B
</td>
</tr>
<tr>
<td>  
Fig 4
</td>
<td>  
File move and safe deletion
</td>
<td>  
Lab 3 — 3B
</td>
</tr>
<tr>
<td>  
Fig 5
</td>
<td>  
Wildcard and find comparison
</td>
<td>  
Lab 3 — 3C
</td>
</tr>
<tr>
<td>  
Fig 6
</td>
<td>  
File discovery by size and date
</td>
<td>  
Lab 3 — 3C
</td>
</tr>
<tr>
<td>  
Fig 7
</td>
<td>  
Tar archive creation and inspection
</td>
<td>  
Lab 4 — 4A
</td>
</tr>
<tr>
<td>  
Fig 8
</td>
<td>  
Archive integrity and restore testing
</td>
<td>  
Lab 4 — 4A
</td>
</tr>
<tr>
<td>  
Fig 9
</td>
<td>  
Log file text utilities
</td>
<td>  
Lab 4 — 4B
</td>
</tr>
<tr>
<td>  
Fig 10
</td>
<td>  
Log summary pipeline and redirection
</td>
<td>  
Lab 4 — 4B
</td>
</tr>
</table>


Evidence standard: Screenshots should clearly show the relevant command and
its output. Avoid unnecessary desktop content, unrelated applications, or
cropped output that removes important context.

## 🎯 Learning Outcomes

By completing Week 02, I developed practical ability to:
- Build organised project workspaces with nested directory structures.
- Manage files safely through creation, copying, editing, comparison, movement, and deletion.
- Use shell wildcards for quick filename matching.
- Use `find` to locate files recursively by name, size, type, and date.
- Create standard tar archives and gzip-compressed archives.
- Inspect archive contents before extraction.
- Generate and preserve SHA-256 checksums for integrity verification.
- Extract archives into a separate directory and verify a clean restore.
- Process and summarise log data using standard Linux text utilities.
- Chain commands with pipelines to transform data efficiently.
- Apply `>` and `>>` correctly to create or append output safely.
- Recognise the importance of checksums and controlled evidence handling in digital forensics.  
These skills support later cybersecurity, incident response, security
operations, evidence preservation, and digital forensics activities.

## 🔐 Evidence and Safety

All exercises were performed in a controlled virtual laboratory environment.
- File creation, modification, archiving, extraction, and deletion were restricted to the designated practice workspace.
- Interactive deletion with `rm -i` was used to reduce the risk of accidental removal.
- Archive contents were inspected and restored in a separate test directory before being treated as verified.
- SHA-256 checksums were generated to support integrity verification.
- File transfer troubleshooting was performed only between the authorised Kali Linux VM and its host system.
- The VirtualBox network adapter was returned to NAT mode after the temporary transfer exercise.
- Screenshots are used as evidence of practical execution.
- Commands are documented so the work can be reviewed and reproduced.

## 📌 Disclaimer

This repository is intended for educational and academic purposes only. All
practical activities were performed in controlled laboratory environments
using virtual machines and test data. No unauthorised system or third-party
infrastructure was targeted.

## 📚 Reference

ICDFA. (2026). WADF-2026-M01 Student Laboratory Workbook. Week 02: Managing
Files, Archiving, Compression and Text Processing. International
Cybersecurity and Digital Forensics Academy.
