# Week 04: Network Configuration, Linux Security, User Administration and Capstone Challenges

## 👤 Author

**Emmanuel Adie Ushie**  
**Linux Essentials: WADF-2026-M01**  
**International Cybersecurity and Digital Forensics Academy**

## 📘 Overview

This folder contains the hands-on laboratory work completed during Week 04 of the Linux Essentials module at the International Cybersecurity and Digital Forensics Academy (ICDFA). Week 04 also serves as the Month 1 assessment and brings together the Linux administration, Bash scripting, access-control, archiving, networking, and log-analysis skills developed throughout the module.

The work is divided into two reports. Lab 7 documents a read-only network and security posture assessment of the Kali Linux virtual machine. Lab 8 documents the capstone challenges covering user and group administration, secure departmental directories, Bash user onboarding, portable archives, and authentication-log investigation.

The documentation is structured for academic submission and professional GitHub portfolio presentation, with reproducible commands, explanations, findings, troubleshooting notes, and screenshot evidence.

## 📘 Week 04 Learning Areas

### Lab 7: Network Configuration and Security Posture

This lab focuses on establishing a network and security baseline without making unnecessary changes to the system.

Topics covered:
- Network interface and link-state inspection
- IPv4 and IPv6 address identification
- Default-route and gateway verification
- Local hostname and DNS resolution
- Listening TCP and UDP service discovery
- Loopback-only and all-interface exposure classification
- Sudo privilege and group-membership review
- SSH security configuration inspection
- UFW and nftables firewall assessment
- Pending package-update review
- Practical Linux hardening recommendations

📑 **Lab Report:**  
`Lab07_NetworkConfiguration_SecurityPosture_Report.md`

### Lab 8: User Administration and Capstone Challenges

This lab applies Month 1 Linux skills through structured administration, scripting, archiving, and investigation tasks.

Topics covered:
- Linux user and group creation
- Department-based access-control design
- Ownership and permission management
- Setgid and sticky directory permissions
- Confidential-file protection
- Parameterised Bash user-onboarding automation
- Input validation and duplicate detection
- Meaningful script exit codes
- Portable tar archive creation and restoration
- Regular-expression and pipeline-based log analysis
- Standard-output and standard-error redirection
- Investigation report generation

📑 **Lab Report:**  
`Lab08_UserAdministration_CapstoneChallenges_Report.md`

## 🧪 Tools and Environment

- **Kali Linux** — Linux laboratory operating system
- **VirtualBox** — virtualisation platform used to run the Linux VM
- **Bash Shell** — command-line and scripting environment
- **ip, ss, getent, resolvectl** — network configuration and name-resolution inspection
- **sudo, groupadd, useradd, chown, chmod** — user, group, ownership, and permission management
- **UFW, nftables, apt** — firewall and update posture assessment
- **tar** — portable archive creation, inspection, and restoration
- **grep, cut, sort, uniq, awk, tee** — log filtering, extraction, counting, and report generation
- **GitHub** — documentation and academic submission platform

## 📁 Evidence Structure

```text
Week04_LinuxEssentials/
├── README.md
├── Lab07_NetworkConfiguration_SecurityPosture_Report.md
├── Lab08_UserAdministration_CapstoneChallenges_Report.md
└── Screenshots/
    ├── Fig01_CLI_InterfaceAddressRoute_IpBrLink_IpRoute.png
    ├── Fig02_CLI_DnsAndHostsResolution_Resolvectl_EtcHosts_Getent.png
    ├── Fig03_CLI_ListeningServicesAndExposure_SsTulpn_SsLtnp.png
    ├── Fig04_CLI_SudoAndSshPosture_SudoL_GrepSshdConfig.png
    ├── Fig05_CLI_SudoGroupMembership_GetentGroupSudo.png
    ├── Fig06_CLI_FirewallInstall_AptInstallUfw.png
    ├── Fig07_CLI_FirewallStatus_UfwStatusVerbose.png
    ├── Fig08_CLI_UpdatablePackages_AptListUpgradable.png
    ├── Fig09_CLI_LabGroupAndUserSetup_Groupadd_Useradd_Chmod2770.png
    ├── Fig10_CLI_DepartmentNamespaceAndGroups_Mkdir_Groupadd.png
    ├── Fig11_CLI_DepartmentUserCreation_UseraddEngSalesIs.png
    ├── Fig12_CLI_DepartmentOwnershipAndPermissions_Chown_Chmod3770.png
    ├── Fig13_CLI_DepartmentDirectoryVerification_LsLd.png
    ├── Fig14_CLI_ConfidentialFileCreation_TeeChownChmod640.png
    ├── Fig15_CLI_ConfidentialFilePermissionCheck_LsL.png
    ├── Fig16_CLI_FullDepartmentVerification_FindPrintf_Getent.png
    ├── Fig17_CLI_OnboardScriptSuccessRun_SudoOnboardUser.png
    ├── Fig18_CLI_OnboardScriptDuplicateGroupTest_ExitCode2.png
    ├── Fig19_CLI_OnboardScriptDuplicateUserTest_ExitCode3.png
    ├── Fig20_CLI_FakeLogFileCreation_ForLoop_LsSource.png
    ├── Fig21_CLI_TarArchiveCreationWithCFlag_TarCvf.png
    ├── Fig22_CLI_ArchiveContentsListing_TarTf.png
    ├── Fig23_CLI_ArchiveRestoreToBackup_TarXvf.png
    └── Fig24_CLI_RestoreVerification_LsBackup.png
```

## 📸 Screenshot Evidence

| Figures | Evidence | Report Section |
|---|---|---|
| Fig 1 | Interface, address, and route inspection | Lab 7 — 7A |
| Figs 2–3 | DNS resolution and listening services | Lab 7 — 7B |
| Figs 4–8 | Sudo, SSH, firewall, and update posture | Lab 7 — 7C |
| Fig 9 | Lab group, users, and shared-directory permissions | Lab 8 — 8A |
| Figs 10–16 | Department users, groups, directories, and confidential files | Lab 8 — Challenge A |
| Figs 17–19 | User-onboarding script tests and exit codes | Lab 8 — Challenge B |
| Figs 20–24 | Controlled log archive creation and restoration | Lab 8 — Challenge C |

**Evidence standard:** Screenshots should clearly display the relevant command and its output. Avoid unrelated applications, unnecessary desktop content, or cropping that removes important context. Authentication-log analysis evidence may be added after Challenge D if screenshots are available.

## 🎯 Learning Outcomes

By completing Week 04, I developed practical ability to:
- Establish a Linux network baseline using read-only inspection commands.
- Explain the relationship between interfaces, IP addresses, gateways, routes, and DNS.
- Identify listening services and assess their potential exposure.
- Review sudo, SSH, firewall, and update posture.
- Create and verify Linux users and groups.
- Design department-based access controls using ownership and numeric permissions.
- Apply setgid and sticky bits to shared directories.
- Protect confidential files from unauthorised access.
- Write and test a defensive Bash onboarding script.
- Use distinct exit codes to communicate script outcomes.
- Build portable tar archives and verify clean restoration.
- Analyse authentication events using regular expressions and multi-stage pipelines.
- Capture standard error separately from standard output.
- Document technical work using reproducible commands and visual evidence.

These skills provide a practical Linux administration foundation for security operations, incident response, network defence, and digital forensics.

## 🔐 Evidence and Safety

All exercises were performed in a controlled virtual laboratory environment.
- A VirtualBox snapshot named `before-week04-lab8` was created before Lab 8.
- Network inspection was read-only unless a documented firewall installation was required.
- Department names, usernames, IP addresses, and log entries were controlled lab data.
- No real authentication logs or third-party infrastructure were used.
- Permissions were applied only to designated practice directories.
- Commands and screenshots were retained for reproducibility and assessment.

## 📌 Disclaimer

This repository is intended for educational and academic purposes only. All practical activities were performed in controlled laboratory environments using virtual machines and test data. No unauthorised system or third-party infrastructure was targeted.

## 📚 References

- ICDFA. (2026). *WADF-2026-M01 Student Laboratory Workbook — Week 04: Network Configuration, Linux Security, User Administration and Capstone Challenges.* International Cybersecurity and Digital Forensics Academy.
- Linux man-pages Project. (n.d.). *Linux manual pages: ip(8), ss(8), useradd(8), groupadd(8), chmod(1), tar(1), and grep(1).* https://man7.org/linux/man-pages/
- ICDFA. (2026). *WADF 101 Module 1 to Module 6.*

