# Lab 36: Bootloaders (GRUB)

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 08 |
| Lab | Lab 36: Bootloaders (GRUB) |
| Date of Submission | 14/08/2026 |


---

## Executive Summary

This lab inspected GRUB configuration sources and safely regenerated the generated bootloader configuration after a virtual-machine snapshot was created.

---

## Objectives

- Distinguish generated GRUB output from administrator-managed sources.
- Inspect `/etc/default/grub` and `/etc/grub.d/`.
- Regenerate configuration safely with `update-grub`.
- Avoid direct edits to `grub.cfg`.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Week 08 laboratory environment.
- **Primary tools:** `cat`, `ls`, `/etc/default/grub`, `/etc/grub.d/`, `update-grub`.
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

### 36A. Inspect GRUB Configuration

```bash
head -n 40 /boot/grub/grub.cfg
cat /etc/default/grub
ls /etc/grub.d/
```

The generated GRUB file, administrator settings, and generator scripts were reviewed without direct modification.

### 36B. Regenerate the Configuration

```bash
sudo update-grub
```

`update-grub` rebuilt `grub.cfg` from the supported source files and reported detected kernels and boot entries.

<img width="1366" height="662" alt="Fig08_CLI_GrubGeneratedConfig_Head png" src="https://github.com/user-attachments/assets/5c745016-8006-4469-8e86-eddade601d6b" />

<img width="1366" height="662" alt="Fig09_CLI_GrubDefaultAndScripts png" src="https://github.com/user-attachments/assets/b2988e50-ff9b-419d-9bca-3e1d3c3748f3" />


<img width="1366" height="662" alt="Fig10_CLI_UpdateGrub_Regeneration png" src="https://github.com/user-attachments/assets/44886dd3-538e-4d46-aa8f-e0a07eed390c" />


---

## Results and Findings

`/boot/grub/grub.cfg` is generated output and should not be edited directly. Persistent administrative changes belong in `/etc/default/grub` or the appropriate scripts, followed by regeneration.

---

## Challenges and Solutions

| Challenge | Resolution |
|---|---|
| Bootloader changes carry recovery risk. | A VM snapshot was taken and the exercise limited changes to regeneration of the current supported configuration. |

---

## Screenshot Reference
| Figure | Filename | Lab |
|---|---|---|
| Fig 08 | `Fig08_CLI_GrubGeneratedConfig_Head png` | 36 |
| Fig 09 | `Fig09_CLI_GrubDefaultAndScripts png` | 36 |
| Fig 10 | `Fig10_CLI_UpdateGrub_Regeneration png` | 36 |

---

## Recommendations

- Back up relevant settings before changes.
- Use supported source files rather than editing `grub.cfg`.
- Review `update-grub` output for unexpected entries.

---

## Conclusion

Lab 36 developed practical competency in bootloaders (grub). The work combined controlled execution, verification, and professional documentation, supporting future Linux administration, incident-response, and digital-forensics activities.

---

## Safety and Evidence Handling

All activities were completed in an authorised virtual laboratory. Potentially disruptive operations were limited to test data, loopback images, or controlled system administration tasks. Commands and screenshots were retained to support reproducibility.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem Links, Hardware, Boot Process, Packages and Month 2 Assessment.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
