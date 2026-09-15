## 👤 Author

Emmanuel Adie Ushie  
WADF-2026-M02: Introduction to Linux  
International Cybersecurity and Digital Forensics Academy

## Week 07: Standard Text Streams, Processes, Archives and File Permissions

This folder contains the hands-on laboratory work completed during Week 07 of
the Introduction to Linux  module at the International Cybersecurity and
Digital Forensics Academy (ICDFA).  
The week focused on controlling standard input, output, and error streams;
managing foreground and background processes; creating, compressing, listing,
and restoring archives; and applying Linux ownership and permission controls.  
The documentation is structured for academic submission and professional
GitHub portfolio presentation, with reproducible commands, technical
explanations, findings, and screenshot evidence.

## 📘 Week 07 Learning Areas

### Lab 29: Standard Text Streams & Redirection

This lab covers Linux data streams, shell redirection, pipelines, and output
capture.  
Topics covered:
- Standard input, output, and error file descriptors
- Output creation and append redirection
- Input redirection from files
- Separate and combined error capture
- Discarding unwanted output with `/dev/null`
- Multi-stage command pipelines
- Simultaneous display and file capture with `tee`  
📑 Lab Report:
Lab29\_StandardTextStreams\_Redirection\_Report.md

### Lab 30: Managing Processes

This lab covers process discovery, shell job control, signals, priority, and
real-time monitoring.  
Topics covered:
- Process listing with `ps` and `ps aux`
- Background execution and shell job inspection
- Foreground and background job control
- Process suspension and resumption
- Graceful and forced termination signals
- Real-time monitoring with `top`
- Process priority with `nice`
- Parent-child relationships with `pstree`  
📑 Lab Report:
Lab30\_ManagingProcesses\_Report.md

### Lab 31: Archive Commands

This lab focuses on creating, compressing, inspecting, extracting, and managing
archives.  
Topics covered:
- Uncompressed tar archives
- gzip and bzip2 compression
- Archive content inspection
- Full and selective extraction
- Extraction to a specified destination
- Time-based archive selection
- Compression-size comparison
- Safe archive lifecycle management  
📑 Lab Report:
Lab31\_ArchiveCommands\_Report.md

### Lab 32: File Permissions

This lab covers Linux access-control notation, ownership, default permissions,
and special permission bits.  
Topics covered:
- Reading long-listing permission strings
- Symbolic and numeric `chmod` notation
- User and group ownership changes
- Recursive permission operations
- Default permissions and `umask`
- setuid behaviour and limitations
- Sticky-bit protection on shared directories  
📑 Lab Report:
Lab32\_FilePermissions\_Report.md

## 🧪 Tools and Environment
- Kali Linux and Ubuntu — Linux laboratory operating systems
- VirtualBox — virtualisation platform used for the laboratory VMs
- Bash Shell — command-line and stream-control environment
- echo, cat, sort, uniq, and tee — text-stream and pipeline utilities
- ps, jobs, fg, bg, kill, top, nice, and pstree — process-management tools
- tar, gzip, and bzip2 — archive and compression utilities
- chmod, chown, chgrp, umask, and ls — permission and ownership tools
- GitHub — documentation and academic submission platform

## 📁 Evidence Structure

Week07\_LinuxEssentials/
├── README.md
├── Lab29\_StandardTextStreams\_Redirection\_Report.md
├── Lab30\_ManagingProcesses\_Report.md
├── Lab31\_ArchiveCommands\_Report.md
├── Lab32\_FilePermissions\_Report.md
└── Screenshots/
├── Fig01\_CLI\_StdoutRedirectionAndAppend\_EchoRedirect\_AppendRedirect.png
├── Fig02\_CLI\_StderrCaptureCombinedStreamsAndPipelines\_2Redirect\_Tee.png
├── Fig03\_CLI\_ProcessListing\_Ps\_PsAux.png
├── Fig04\_CLI\_BackgroundJobsAndSuspend\_SleepAmp\_Jobs\_CtrlZ.png
├── Fig05\_CLI\_JobControlAndKillSignals\_Bg\_Kill\_KillNine.png
├── Fig06\_CLI\_TopMonitorNiceAndProcessTree\_Top\_Nice\_Pstree.png
├── Fig07\_CLI\_ArchiveTestSetupAndTarCreation\_MkdirTouch\_TarCvf.png
├── Fig08\_CLI\_ArchiveCompressionFormats\_Gzip\_TarCzf\_TarCjf.png
├── Fig09\_CLI\_ArchiveExtractionAndIncrementalBackup\_TarXvf\_NewerMtimeThan.png
├── Fig10\_CLI\_PermissionReadingAndSymbolicChmod\_LsL\_ChmodUx.png
├── Fig11\_CLI\_NumericChmodAndOwnershipChange\_Chmod755\_Chown\_Chgrp.png
└── Fig12\_CLI\_RecursivePermissionsUmaskAndSpecialBits\_ChmodR\_Umask\_Setuid\_Sticky.png

## 📸 Screenshot Evidence

Screenshots are stored in `Screenshots/` and embedded immediately after the
commands or activities they verify.

| Figure | Evidence | Report Section |
|---|---|---|
| Fig 1 | Standard output redirection and append | Lab 29 |
| Fig 2 | Error capture, combined streams, pipelines, and `tee` | Lab 29 |
| Fig 3 | Process listing | Lab 30 |
| Fig 4 | Background jobs and suspension | Lab 30 |
| Fig 5 | Job control and termination signals | Lab 30 |
| Fig 6 | Monitoring, priority, and process tree | Lab 30 |
| Fig 7 | Test workspace and tar creation | Lab 31 |
| Fig 8 | gzip and bzip2 archive formats | Lab 31 |
| Fig 9 | Extraction and time-based archive selection | Lab 31 |
| Fig 10 | Permission reading and symbolic changes | Lab 32 |
| Fig 11 | Numeric permissions and ownership | Lab 32 |
| Fig 12 | Recursive permissions, umask, and special bits | Lab 32 |

Evidence standard: Screenshots should show the relevant command and complete
output while excluding unrelated applications or unnecessary desktop content.

## 🎯 Learning Outcomes

By completing Week 07, I developed practical ability to:
- Distinguish standard input, standard output, and standard error.
- Redirect, append, combine, discard, and pipeline command output safely.
- Capture visible output and evidence files with `tee`.
- Discover and control foreground and background processes.
- Apply termination signals according to process behaviour.
- Monitor resource usage and assign lower scheduling priority.
- Create and inspect compressed and uncompressed archives.
- Restore complete archives and selected entries to controlled locations.
- Interpret Linux file types and permission strings.
- Apply symbolic, numeric, ownership, and default permission settings.
- Explain setuid and sticky-bit behaviour and associated security risks.  
These skills support Linux administration, incident response, evidence
collection, access-control review, and digital-forensics workflows.

## 🔐 Evidence and Safety

All exercises were performed in controlled virtual laboratory environments.
- Stream and pipeline activities used test files in the authorised workspace.
- Process-control exercises used disposable `sleep` processes.
- Archives contained laboratory test files and were restored separately.
- Permission and ownership changes were limited to designated training data.
- Forced termination and special permission bits were used only for demonstration.
- Screenshots and commands were retained for reproducibility.

## 📌 Disclaimer

This repository is intended for educational and academic purposes only. All
activities were performed in controlled virtual machines using test data. No
unauthorised system or third-party infrastructure was targeted.

## 📚 Reference

ICDFA. (2026). WADF-2026-M02 Week 7 Practical Labs: Labs 29–32, Standard Text
Streams, Processes, Archives and File Permissions. International
Cybersecurity and Digital Forensics Academy.
