LAB REPORT: LINUX SETUP AND SHELL FUNDAMENTALS

STUDENT INFORMATION
--------------------------------------------------------------------------
| Field | Details |  
|---|---|
| Student Name | Emmanuel Adie Ushie   
|Course Name | Linux Essentials — WADF-2026-M01  
|Instructor | Mr. Udam Akume Gabriel  
|Week | Week 01  
|Lab | Lab 1 — Linux Setup & Shell Fundamentals  
|Date of Submission | 26/06/2026
--------------------------------------------------------------------------

 
1. EXECUTIVE SUMMARY

This laboratory exercise established and validated the Linux environment
required for subsequent cybersecurity practical work.

The lab covered system verification, command-line orientation, command
exit status codes, command history, and the use of built-in Linux help
and manual pages. The exercises were completed in a Kali Linux virtual
machine running through VirtualBox.

The objective was not only to execute commands, but also to understand
what each command does, interpret its output, and document the work in
a reproducible manner.

---   
2. LAB OBJECTIVES  


- Validate the Linux virtual machine and confirm that it is ready for
  laboratory work.
- Practise fundamental terminal commands.
- Understand command exit status codes.
- Identify the current user, shell, working directory, and system
  information.
- Use Linux's built-in help and manual systems.
- Distinguish between built-in shell commands and external commands.
- Preserve command history as supporting evidence.
---
 
3. TOOLS AND ENVIRONMENT  

- Kali Linux
- VirtualBox
- Bash Shell
- Linux manual pages and built-in help utilities
- VirtualBox Snapshot
---
 
4. METHODOLOGY  


------------------------------------------------------------
4.1 Linux VM Validation
------------------------------------------------------------

The Linux virtual machine was first validated to confirm the operating
system, kernel, hostname, storage devices, and available disk capacity.

Commands Executed:

    hostnamectl
    uname -r
    cat /etc/os-release
    lsblk
    df -h /

Purpose:

- hostnamectl          verifies hostname and operating-system information.
- uname -r              identifies the running Linux kernel version.
- cat /etc/os-release   confirms the Linux distribution and version.
- lsblk                 displays attached storage devices and partitions.
- df -h /               checks available storage space on the root
                        filesystem.

📸 Screenshot Evidence — Figure 01
Filename:<img width="1366" height="662" alt="Fig01_CLI_SystemVerification_Hostnamectl_Uname_OsRelease png" src="https://github.com/user-attachments/assets/a52d7336-4ec6-4edd-bd95-0191031c4dc5" />


📸 Screenshot Evidence — Figure 02
Filename: <img width="1366" height="662" alt="Fig02_CLI_DiskAndStorage_LsbDfH pnglk" src="https://github.com/user-attachments/assets/164ca4e2-1854-425e-9710-ea48e6cac9ec" />


------------------------------------------------------------
4.2 Command-Line Orientation
------------------------------------------------------------

Basic commands were executed to understand the current user, user
groups, working directory, active shell, system date/time, and command
history.

Commands Executed:

    whoami
    id
    pwd
    echo "$SHELL"
    date
    history | tail -n 25

The following commands were also used to demonstrate successful and
unsuccessful command execution:

    ls ~
    echo $?

    ls ~/this-file-does-not-exist.txt
    echo $?

The command history was then exported:

    history | tail -n 25 > ~/week1_command_history.txt

Key Observation:
The exit status exercise demonstrated that `echo $?` reports the status
of the immediately preceding command. A successful command returns 0,
while a failed command returns a non-zero value.

📸 Screenshot Evidence — Figure 03
Filename: <img width="1366" height="662" alt="Fig03_CLI_UserAndShellOrientation_Whoami_Id_Pwd_Shell png" src="https://github.com/user-attachments/assets/55672e03-9e00-471b-bdd7-77542ed3b999" />


📸 Screenshot Evidence — Figure 04
Filename: <img width="1366" height="662" alt="Fig04_CLI_ExitStatusAndHistoryExport png" src="https://github.com/user-attachments/assets/365518ed-3337-45ad-8b53-9340819d4ca6" />


------------------------------------------------------------
4.3 Built-In Help and Manual Pages
------------------------------------------------------------

Linux provides several ways to obtain command documentation without
relying on external sources.

Commands Executed:

    ls --help | head -n 20
    man pwd
    help cd
    apropos archive
    whatis tar
    command -V ls
    command -V cd

Help Methods Practised:
--------------------------------------------------------------------------
Command   Type       Help Method          Example Option / Information
--------------------------------------------------------------------------
ls        External   man ls / ls --help   -a, -l
cd        Built-in   help cd              -, ~
tar       External   man tar              -x, -z
--------------------------------------------------------------------------

Key Observation:
The exercises demonstrated that Linux provides multiple built-in
documentation methods. The `command -V` check also showed that `ls` is
an external command while `cd` is implemented as a shell built-in.

📸 Screenshot Evidence — Figure 05
Filename: <img width="1366" height="662" alt="Fig05_CLI_HelpAndManPages_LsHelp_ManPwd_HelpCd_Apropos png" src="https://github.com/user-attachments/assets/200fcb6c-a174-4305-bcaf-992634b126b2" />

---

5. FINDINGS

The laboratory confirmed that the Kali Linux environment was ready for
practical work and provided a working foundation for subsequent labs.

The command-line orientation exercises established an understanding of
how Linux identifies the current user, shell, working directory, and
system state.

The exit-status exercise was particularly useful because it provided an
objective way to determine whether a command completed successfully.
The built-in documentation exercises also demonstrated that
troubleshooting can begin directly from the operating system using
`man`, `--help`, `help`, `apropos`, and `whatis`.


6. CHALLENGES AND SOLUTIONS

--------------------------------------------------------------------------
Challenge                                  Solution
--------------------------------------------------------------------------
Permission-related administration tasks    Used sudo where administrator
                                            privileges were required.

Difficulty remembering command syntax      Used man and --help to verify
                                            syntax and available options.

Incorrect interpretation of exit status    Learned to run echo $?
                                            immediately after the target
                                            command.

Case sensitivity in Linux commands and     Repeated commands carefully
options                                     and verified syntax using
                                            built-in documentation.
--------------------------------------------------------------------------

7. LEARNING OUTCOMES

After completing Lab 1, I was able to:

- Validate a Linux virtual machine.
- Identify operating-system and kernel information.
- Check storage devices and disk capacity.
- Identify the current user and shell.
- Navigate the command line with greater confidence.
- Interpret command exit status codes.
- Save command history as evidence.
- Use multiple Linux help and documentation methods.
- Identify whether commands are shell built-ins or external programs.

8. CONCLUSION

Lab 1 established the basic Linux command-line and system-verification
skills required for the remainder of the Linux Essentials module.

The practical work moved beyond simply memorising commands by focusing
on verification, interpretation, documentation, and safe laboratory
practice. These skills form an important foundation for later
cybersecurity and digital forensics exercises.

9. REFERENCE

ICDFA. (2026). WADF-2026-M01 Student Laboratory Workbook — Week 01:
Linux Setup, Shell Fundamentals and Filesystem Navigation. International
Cybersecurity and Digital Forensics Academy.
