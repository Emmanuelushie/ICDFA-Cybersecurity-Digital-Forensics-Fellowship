# Lab 41: Package Management

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 08 |
| Lab | Lab 41: Package Management |
| Date of Submission | 14/08/2026 |


---

## Executive Summary

This lab covered the Debian package lifecycle: counting installed packages, searching metadata, inspecting a package, refreshing indexes, installing and verifying a utility, listing installed files, and removing the package.

---

## Objectives

- Search and inspect package metadata.
- Refresh repository indexes.
- Install and verify a package.
- List package-owned files.
- Remove a package and review unused dependencies.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Week 08 laboratory environment.
- **Primary tools:** `apt`, `apt-cache`, `dpkg`, `grep`, `wc`.
- **VirtualBox:** Provided isolated hardware and a recoverable training platform.
- **GitHub:** Portfolio and academic documentation platform.

---

## Workspace and Evidence Storage

```text
/media/sf_ICDFA/Week08/
/media/sf_ICDFA/Week08/Screenshots/
/media/sf_ICDFA/Week08/Notes/
```

---

## Methodology

### 41A. Search and Inspect Packages

```bash
apt list --installed 2>/dev/null | wc -l
apt-cache search "text editor" | head -n 10
apt-cache show curl | head -n 15
```

The package database and local metadata were queried before any installation.

### 41B. Install, Verify, and Remove a Test Package

```bash
sudo apt update
sudo apt install -y tree
dpkg -l | grep '^ii.*tree'
dpkg -L tree | head -n 10
sudo apt remove -y tree
sudo apt autoremove -y
```

APT resolved dependencies and managed repositories, while `dpkg` verified package status and listed files registered to the package.

<img width="1366" height="662" alt="Fig20_CLI_PackageSearch_AptCache png" src="https://github.com/user-attachments/assets/7f1ce18d-183a-409c-9058-80517b752d49" />


<img width="1366" height="662" alt="Fig21_CLI_AptUpdate_InstallTree png" src="https://github.com/user-attachments/assets/7d4979d8-3314-49f9-ac33-0ddc50e17c9f" />


<img width="1366" height="662" alt="Fig22_CLI_DpkgVerification_FileList png" src="https://github.com/user-attachments/assets/e75c2d70-e4e0-420a-91f0-f3bd771eb0dc" />


<img width="1366" height="662" alt="Fig23_CLI_AptRemove_Autoremove png" src="https://github.com/user-attachments/assets/c8555144-71cf-4030-8e42-9e432258b727" />


---

## Results and Findings

APT is a high-level package-management interface that works with repositories and dependency resolution. `dpkg` provides lower-level inspection and package-database operations. `apt remove` may retain package configuration, while purge is used when configuration removal is intended.

---

## Challenges and Solutions

| Challenge | Resolution |
|---|---|
| Repository refresh required network access. | The configured repositories and connectivity were verified before installation; no package result was fabricated when a repository was unavailable. |

---

## Screenshot Reference
| Figure | Filename | Lab |
|---|---|---|
| Fig 20 | `Fig20_CLI_PackageSearch_AptCache.png` | 41 |
| Fig 21 | `Fig21_CLI_AptUpdate_InstallTree.png` | 41 |
| Fig 23 | `Fig23_CLI_AptRemove_Autoremove.png` | 41 |
| Fig 24 | `Fig23_CLI_AptRemove_Autoremove.png` | 41 |
---

## Recommendations

- Run package operations only from trusted repositories.
- Review `autoremove` proposals before approval on important systems.
- Capture package versions during incident response.

---

## Conclusion

Lab 41 developed practical competency in package management. The work combined controlled execution, verification, and professional documentation, supporting future Linux administration, incident-response, and digital-forensics activities.

---

## Safety and Evidence Handling

All activities were completed in an authorised virtual laboratory. Potentially disruptive operations were limited to test data, loopback images, or controlled system administration tasks. Commands and screenshots were retained to support reproducibility.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem Links, Hardware, Boot Process, Packages and Month 2 Assessment.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
