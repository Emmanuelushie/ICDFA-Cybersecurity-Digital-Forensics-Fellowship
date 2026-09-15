# Lab 38: Mounting Filesystems

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 08 |
| Lab | Lab 38: Mounting Filesystems |
| Date of Submission | 14/08/2026 |


---

## Executive Summary

This lab practised filesystem inspection, creation, mounting, and unmounting with a 100 MB loopback image so that no real disk partition was modified.

---

## Objectives

- Inspect filesystems and current mounts.
- Create and format a loopback image.
- Mount and unmount the image safely.
- Review persistent-mount configuration.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Week 08 laboratory environment.
- **Primary tools:** `lsblk -f`, `df`, `mount`, `umount`, `dd`, `mkfs.ext4`, `/etc/fstab`.
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

### 38A. Inspect Existing Storage

```bash
lsblk -f
df -h
mount | column -t
cat /etc/fstab
```

These commands displayed filesystem types, identifiers, capacity, mount points, options, and persistent configuration.

### 38B. Create and Mount a Loopback Filesystem

```bash
dd if=/dev/zero of=~/testdisk.img bs=1M count=100
sudo mkfs.ext4 ~/testdisk.img
sudo mkdir -p /mnt/test-mount
sudo mount -o loop ~/testdisk.img /mnt/test-mount
df -h /mnt/test-mount
sudo umount /mnt/test-mount
```

A blank image was formatted as ext4, mounted through the loop driver, verified, and cleanly unmounted.


<img width="1366" height="662" alt="Fig14_CLI_FilesystemInspection_Lsblk_Df_Mount png" src="https://github.com/user-attachments/assets/751af02d-8fb0-464d-8e95-1c46f628cb84" />


<img width="1366" height="662" alt="Fig15_CLI_LoopImage_Dd_MkfsExt4 png" src="https://github.com/user-attachments/assets/d77b72f7-d5c3-42a8-95bf-97cea4407746" />


<img width="1366" height="662" alt="Fig16_CLI_LoopMount_Umount_Fstab png" src="https://github.com/user-attachments/assets/9a59c9ce-db3b-4e01-afe1-5904b97e86d9" />

---

## Results and Findings

Mounting attaches a filesystem to the single Linux directory tree. UUID-based references are generally more stable than `/dev/sdX` names, which may change across boots or device-order changes.

---

## Challenges and Solutions

| Challenge | Resolution |
|---|---|
| The mount point did not initially exist. | `sudo mkdir -p /mnt/test-mount` was run before mounting. |

---

## Screenshot Reference
| Figure | Filename | Lab |
|---|---|---|
| Fig 14 | `Fig14_CLI_FilesystemInspection_Lsblk_Df_Mount.png` | 38 |
| Fig 15 | `Fig15_CLI_LoopImage_Dd_MkfsExt4.png` | 38 |
| Fig 16 | `Fig16_CLI_LoopMount_Umount_Fstab.png` | 38 |
---

## Recommendations

- Confirm the target device or image before `mkfs`.
- Unmount cleanly before integrity checks.
- Back up `/etc/fstab` and validate entries before reboot.

---

## Conclusion

Lab 38 developed practical competency in mounting filesystems. The work combined controlled execution, verification, and professional documentation, supporting future Linux administration, incident-response, and digital-forensics activities.

---

## Safety and Evidence Handling

All activities were completed in an authorised virtual laboratory. Potentially disruptive operations were limited to test data, loopback images, or controlled system administration tasks. Commands and screenshots were retained to support reproducibility.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem Links, Hardware, Boot Process, Packages and Month 2 Assessment.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
