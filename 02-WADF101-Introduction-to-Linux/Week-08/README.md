## 👤 Author

Emmanuel Adie Ushie  
WADF-2026-M02: Introduction to Linux  
International Cybersecurity and Digital Forensics Academy

## Week 08: Filesystem Links, Hardware, Boot, Storage, Packages and Libraries

This folder contains the hands-on laboratory work completed during Week 08 of
the Introduction to Linux I module at the International Cybersecurity and
Digital Forensics Academy (ICDFA).  
The week covered filesystem links, hardware discovery, Linux boot analysis,
GRUB, systemd targets, loopback filesystem mounting, integrity checking,
controlled repair, package management, and shared-library resolution. It also
formed part of the Month 2 assessment and consolidated skills developed across
the module.  
The documentation is structured for academic submission and professional
GitHub portfolio presentation, with reproducible commands, technical
explanations, findings, and screenshot evidence.

## 📘 Week 08 Learning Areas

### Lab 33: Filesystem Links
  
Topics covered:
- Explain the relationship between filenames and inodes.
- Create and verify hard links and symbolic links.
- Compare inode values and link counts.
- Demonstrate how deletion of a target affects each link type.  
📑 Lab Report:
Lab33_FilesystemLinks_Report.md

### Lab 34: Hardware Configuration
  
Topics covered:
- Collect CPU and memory information.
- Identify block, USB, and PCI devices.
- Review firmware-provided system information.
- Distinguish guest-visible virtual hardware from physical host hardware.  
📑 Lab Report:
Lab34_HardwareConfiguration_Report.md

### Lab 35: The Boot Process
  
Topics covered:
- Measure major boot phases.
- Identify slow-starting services.
- Review the critical dependency chain.
- Inspect current-boot and kernel messages.  
📑 Lab Report:
Lab35_TheBootProcess_Report.md

### Lab 36: Bootloaders (GRUB)
  
Topics covered:
- Distinguish generated GRUB output from administrator-managed sources.
- Inspect `/etc/default/grub` and `/etc/grub.d/`.
- Regenerate configuration safely with `update-grub`.
- Avoid direct edits to `grub.cfg`.  
📑 Lab Report:
Lab36_Bootloaders(GRUB)_Report.md

### Lab 37: Runlevels and systemd Targets
  
Topics covered:
- Identify the default systemd target.
- List active targets.
- Relate legacy runlevels to modern targets.
- Inspect target dependencies.  
📑 Lab Report:
Lab37_RunlevelsandsystemdTargets_Report.md

### Lab 38: Mounting Filesystems
  
Topics covered:
- Inspect filesystems and current mounts.
- Create and format a loopback image.
- Mount and unmount the image safely.
- Review persistent-mount configuration.  
📑 Lab Report:
Lab38_MountingFilesystems_Report.md

### Lab 39: Maintaining Filesystem Integrity
  
Topics covered:
- Preview and run filesystem checks safely.
- Review ext-filesystem metadata.
- Scan an image for unreadable blocks.
- Understand SMART limitations in virtual machines.  
📑 Lab Report:
Lab39_MaintainingFilesystemIntegrity_Report.md

### Lab 40: Fixing Filesystems
  
Topics covered:
- Preserve a clean baseline image.
- Check a working copy before repair.
- Run controlled automatic repair.
- Verify the filesystem with a second pass.  
📑 Lab Report:
Lab40_FixingFilesystems_Report.md

### Lab 41: Package Management
  
Topics covered:
- Search and inspect package metadata.
- Refresh repository indexes.
- Install and verify a package.
- List package-owned files.
- Remove a package and review unused dependencies.  
📑 Lab Report:
Lab41_PackageManagement_Report.md

### Lab 42: Managing Shared Libraries
  
Topics covered:
- Inspect a program’s shared-library dependencies.
- Review linker configuration and drop-in files.
- Query the linker cache.
- Rebuild the cache in a controlled environment.
- Inspect `LD_LIBRARY_PATH`.  
📑 Lab Report:
Lab42_ManagingSharedLibraries_Report.md

## 🧪 Tools and Environment
- Kali Linux — Linux laboratory operating system
- VirtualBox — isolated virtualisation and snapshot environment
- Bash Shell — command-line environment
- ln and ls — links and inode inspection
- lscpu, free, lsblk, lsusb, lspci, and dmidecode — hardware inventory
- systemd-analyze, journalctl, dmesg, systemctl, and GRUB tools — boot analysis
- dd, mkfs.ext4, mount, fsck, tune2fs, and badblocks — filesystem laboratories
- apt, apt-cache, and dpkg — package management
- ldd and ldconfig — shared-library inspection and cache management
- GitHub — documentation and portfolio platform

## 📁 Evidence Structure

Week08_LinuxEssentials/
├── README.md
├── Lab33_FilesystemLinks_Report.md
├── Lab34_HardwareConfiguration_Report.md
├── Lab35_TheBootProcess_Report.md
├── Lab36_BootloadersGRUB_Report.md
├── Lab37_RunlevelsSystemdTargets_Report.md
├── Lab38_MountingFilesystems_Report.md
├── Lab39_MaintainingFilesystemIntegrity_Report.md
├── Lab40_FixingFilesystems_Report.md
├── Lab41_PackageManagement_Report.md
├── Lab42_ManagingSharedLibraries_Report.md
└── Screenshots/

## 📸 Screenshot Evidence

| Lab | Evidence Area | Location |
|---|---|---|
| Lab 33 | Filesystem Links | Embedded evidence in the Lab 33 report |
| Lab 34 | Hardware Configuration | Embedded evidence in the Lab 34 report |
| Lab 35 | The Boot Process | Embedded evidence in the Lab 35 report |
| Lab 36 | Bootloaders (GRUB) | Embedded evidence in the Lab 36 report |
| Lab 37 | Runlevels and systemd Targets | Embedded evidence in the Lab 37 report |
| Lab 38 | Mounting Filesystems | Embedded evidence in the Lab 38 report |
| Lab 39 | Maintaining Filesystem Integrity | Embedded evidence in the Lab 39 report |
| Lab 40 | Fixing Filesystems | Embedded evidence in the Lab 40 report |
| Lab 41 | Package Management | Embedded evidence in the Lab 41 report |
| Lab 42 | Managing Shared Libraries | Embedded evidence in the Lab 42 report |

Evidence standard: Screenshots should show the relevant command and complete
output, while excluding unrelated applications and unnecessary desktop content.

## 🎯 Learning Outcomes

By completing Week 08, I developed practical ability to:
- Distinguish hard links from symbolic links using inode evidence.
- Collect a read-only virtual-machine hardware inventory.
- Analyse boot timing, dependencies, journal records, and kernel messages.
- Inspect and safely regenerate GRUB configuration.
- Relate traditional runlevels to systemd targets.
- Create, mount, verify, and unmount loopback filesystems.
- Check filesystem integrity and follow a copy-first repair workflow.
- Search, install, verify, and remove Debian packages.
- Inspect shared-library dependencies and linker configuration.
- Apply integrated Linux skills in assessment scenarios.  
These skills support Linux administration, system troubleshooting, live
response, evidence preservation, and digital-forensics analysis.

## 🔐 Evidence and Safety

All exercises were performed in a controlled virtual laboratory environment.
- Destructive storage operations were restricted to loopback image files.
- The original filesystem image was preserved before repair testing.
- GRUB activities were preceded by a virtual-machine snapshot.
- Hardware and boot inspection commands were read-only.
- Package changes used configured repositories and a small test package.
- Screenshots and command output were retained for reproducibility.

## 📌 Disclaimer

This repository is intended for educational and academic purposes only. All
activities were performed in controlled virtual machines using test data. No
unauthorised system or third-party infrastructure was targeted.

## 📚 Reference

ICDFA. (2026). WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem
Links, Hardware, Boot Process, Packages and Month 2 Assessment. International
Cybersecurity and Digital Forensics Academy.

