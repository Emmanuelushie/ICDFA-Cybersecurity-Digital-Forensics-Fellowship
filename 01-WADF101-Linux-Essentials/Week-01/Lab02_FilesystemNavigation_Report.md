LAB REPORT: FILESYSTEM NAVIGATION

STUDENT INFORMATION
--------------------------------------------------------------------------
| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie 
| Course Name | Linux Essentials — WADF-2026-M01 
| Instructor | Mr. Udam Akume Gabriel
| Week | Week 01
| Lab | Lab 2 — Filesystem Navigation
| Date of Submission | 03/06/2026
--------------------------------------------------------------------------

1. EXECUTIVE SUMMARY

This laboratory exercise focused on understanding and navigating the
Linux filesystem.

The practical work covered absolute and relative paths, directory
creation, filesystem tree discovery, exploration of important system
directories, directory permissions, file creation, hidden files, file
discovery, file identification, and safe deletion.

All file-management activities were performed inside a dedicated
practice workspace to reduce the risk of modifying important
operating-system files.
---  

2. LAB OBJECTIVES

- Navigate the Linux filesystem using absolute and relative paths.
- Create and manage a dedicated practice directory.
- Understand the purpose of important Linux system directories.
- Inspect directory ownership and permissions.
- Create, list, search, and identify files.
- Work with hidden files.
- Practise safe deletion and workspace hygiene.
---  

3. TOOLS AND ENVIRONMENT

- Kali Linux
- VirtualBox
- Bash Shell
- Linux filesystem utilities
- Dedicated practice workspace: ~/wadf-labs/week1/
---  

4. METHODOLOGY

------------------------------------------------------------
4.1 Absolute and Relative Path Navigation
------------------------------------------------------------

A dedicated laboratory workspace was created for Week 01:

    mkdir -p ~/wadf-labs/week1/{notes,evidence,sandbox}

Navigation was then practised using both absolute and relative paths:

    cd ~/wadf-labs/week1/sandbox
    pwd

    cd ..
    cd notes

    cd ~/wadf-labs/week1/sandbox

    cd ~

The directory tree was verified with:

    find ~/wadf-labs/week1 -maxdepth 2 -type d | sort

Key Observation:
An absolute path provides the complete path from the root of the
filesystem, while a relative path depends on the current working
directory.

📸 Screenshot Evidence — Figure 06
Filename: <img width="1366" height="662" alt="Fig06_CLI_PathNavigation_FindWeek1DirectoryTree png" src="https://github.com/user-attachments/assets/0f3d0ed0-692c-4acd-b0a5-21ca9efc6ac8" />


------------------------------------------------------------
4.2 System Directory Discovery
------------------------------------------------------------

The top-level Linux filesystem was explored without intentionally
modifying system files.

Commands Executed:

    ls -la /
    ls -ld /home /tmp /etc
    find /etc -maxdepth 1 -type f | head -n 10
    find /var/log -maxdepth 1 -type f 2>/dev/null | head -n 10
    stat /tmp

Important Directories:
--------------------------------------------------------------------------
Directory   Purpose
--------------------------------------------------------------------------
/etc        Contains system-wide configuration files.
/var        Contains files that change over time, including logs and
            caches.
/home       Contains users' personal directories.
/tmp        Provides temporary storage for short-lived files.
/usr        Contains many installed programs and shared resources.
/bin        Contains essential command-line utilities.
--------------------------------------------------------------------------

Key Observation:
The exploration demonstrated that the Linux filesystem is organised
according to specific system functions rather than as an arbitrary
collection of folders.

The `stat /tmp` exercise also provided an opportunity to observe
directory metadata and special permission behaviour.

📸 Screenshot Evidence — Figure 07
Filename: <img width="1366" height="662" alt="Fig07_CLI_SystemDirectoryDiscovery_LsLa_LsLd_Stat png" src="https://github.com/user-attachments/assets/c16b4ead-01a3-442b-a352-36386f7b35ab" />


------------------------------------------------------------
4.3 File Discovery and Safe Workspace Hygiene
------------------------------------------------------------

Sample files were created inside the sandbox directory:

    touch ~/wadf-labs/week1/sandbox/{alpha.txt,beta.txt,gamma.txt}

A hidden file was also created:

    touch ~/wadf-labs/week1/sandbox/.hidden-note

The files were then listed:

    ls -lah ~/wadf-labs/week1/sandbox

Text files were discovered using:

    find ~/wadf-labs/week1 -type f -name "*.txt"

File types were examined using:

    file ~/wadf-labs/week1/sandbox/*

Finally, a clearly identified disposable file was created and removed:

    touch ~/wadf-labs/week1/sandbox/DELETE_ME.txt
    rm ~/wadf-labs/week1/sandbox/DELETE_ME.txt

Key Observation:
The exercise reinforced the importance of using a controlled workspace
when practising file-management commands. It also demonstrated how
hidden files, filename patterns, and file types can be identified from
the command line.

📸 Screenshot Evidence — Figure 08
Filename: <img width="1366" height="662" alt="Fig08_CLI_FileDiscoveryAndCleanup_TouchFindFile_Rm png" src="https://github.com/user-attachments/assets/b501dd17-2a2d-4568-85bc-966bb01447ac" />

---
5. FINDINGS

The navigation exercises demonstrated the practical difference between
absolute and relative paths. Absolute paths provide a complete
location, while relative paths make navigation shorter when the current
directory is known.

The system-directory exercises showed that major Linux directories have
distinct roles. Configuration, logs, user data, temporary files,
programs, and essential utilities are organised into different
filesystem locations.

The file-discovery exercises demonstrated practical use of `ls`,
`find`, and `file`, while the controlled deletion exercise reinforced
safe workspace hygiene.

---
6. CHALLENGES AND SOLUTIONS

--------------------------------------------------------------------------
Challenge                                  Solution
--------------------------------------------------------------------------
Adjusting to Linux filesystem structure    Practised navigation and used
                                            pwd and find to verify
                                            location.

Confusion between absolute and relative    Repeated navigation using
paths                                       both methods and compared
                                            the results.

Identifying hidden files                   Used ls -lah to display
                                            hidden entries.

Risk of deleting the wrong file            Restricted deletion practice
                                            to a dedicated sandbox and
                                            used a clearly named
                                            disposable file.

Understanding directory permissions        Compared permissions with
                                            ls -ld and inspected
                                            metadata with stat.
--------------------------------------------------------------------------

7. LEARNING OUTCOMES

After completing Lab 2, I was able to:

- Navigate Linux directories using absolute and relative paths.
- Create a structured practice workspace.
- Inspect the Linux filesystem hierarchy.
- Identify the purpose of important system directories.
- Examine directory permissions and metadata.
- Create and identify hidden files.
- Search for files using find.
- Identify file types using file.
- Practise safe file deletion within a controlled workspace.
---

8. CONCLUSION

Lab 2 provided practical experience with the Linux filesystem and
established a safer approach to navigating, inspecting, and managing
files.

Understanding filesystem structure, permissions, paths, logs, and file
discovery is particularly important for future cybersecurity and
digital forensics work, where accurate navigation and evidence handling
are essential.

---
9. REFERENCE

ICDFA. (2026). WADF-2026-M01 Student Laboratory Workbook — Week 01:
Linux Setup, Shell Fundamentals and Filesystem Navigation. International
Cybersecurity and Digital Forensics Academy.
