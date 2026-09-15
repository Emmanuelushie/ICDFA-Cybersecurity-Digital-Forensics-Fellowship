# Lab 39: Maintaining Filesystem Integrity

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 08 |
| Lab | Lab 39: Maintaining Filesystem Integrity |
| Date of Submission | 14/08/2026 |


---

## Executive Summary

This lab performed proactive, read-only or controlled integrity checks against an unmounted ext4 loopback image and reviewed filesystem metadata and virtual-disk limitations.

---

## Objectives

- Preview and run filesystem checks safely.
- Review ext-filesystem metadata.
- Scan an image for unreadable blocks.
- Understand SMART limitations in virtual machines.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Week 08 laboratory environment.
- **Primary tools:** `fsck`, `tune2fs`, `badblocks`, `smartctl`.
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

### 39A. Check the Unmounted Image

```bash
sudo fsck -N ~/testdisk.img
sudo fsck -f ~/testdisk.img
sudo tune2fs -l ~/testdisk.img | head -n 20
sudo badblocks -sv ~/testdisk.img
sudo smartctl -a /dev/sda
```

`fsck -N` displayed the proposed check command without execution. `fsck -f` forced a full check, `tune2fs` displayed ext metadata, and `badblocks` performed a read-only scan. SMART data was requested from the virtual disk but may not be exposed by the hypervisor.

<img width="1366" height="662" alt="Fig17_CLI_FilesystemIntegrity_Fsck_Tune2fs png" src="https://github.com/user-attachments/assets/9d606ceb-eda1-4831-ae22-97f2d78226f6" />

<img width="1366" height="662" alt="Fig18_CLI_Badblocks_Smartctl png" src="https://github.com/user-attachments/assets/e9f86db7-542a-45aa-9c81-63908a5f131b" />


---

## Results and Findings

Filesystem checks should normally be performed while the target filesystem is unmounted. SMART availability depends on the storage stack and was not assumed in a virtual environment.

---

## Challenges and Solutions

| Challenge | Resolution |
|---|---|
| SMART information was unavailable. | The limitation was documented as a property of the virtual disk presentation rather than treated as a filesystem failure. |

---

## Screenshot Reference
| Figure | Filename | Lab |
|---|---|---|
| Fig 17 | `Fig17_CLI_FilesystemIntegrity_Fsck_Tune2fs.png` | 39 |
| Fig 18 | `Fig18_CLI_Badblocks_Smartctl.png` | 39 |
---

## Recommendations

- Never run repair-mode checks on a mounted root filesystem.
- Work from verified copies where evidence preservation matters.
- Treat `badblocks` write modes as destructive unless explicitly controlled.

---

## Conclusion

Lab 39 developed practical competency in maintaining filesystem integrity. The work combined controlled execution, verification, and professional documentation, supporting future Linux administration, incident-response, and digital-forensics activities.

---

## Safety and Evidence Handling

All activities were completed in an authorised virtual laboratory. Potentially disruptive operations were limited to test data, loopback images, or controlled system administration tasks. Commands and screenshots were retained to support reproducibility.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem Links, Hardware, Boot Process, Packages and Month 2 Assessment.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
