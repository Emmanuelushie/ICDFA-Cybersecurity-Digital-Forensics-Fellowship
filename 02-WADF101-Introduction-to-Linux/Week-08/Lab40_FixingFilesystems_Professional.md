# Lab 40: Fixing Filesystems

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 08 |
| Lab | Lab 40: Fixing Filesystems |
| Date of Submission | 14/08/2026 |


---

## Executive Summary

This lab demonstrated a controlled filesystem-repair workflow on a copy of the loopback image, preserving the original as a baseline and verifying the result after repair.

---

## Objectives

- Preserve a clean baseline image.
- Check a working copy before repair.
- Run controlled automatic repair.
- Verify the filesystem with a second pass.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Week 08 laboratory environment.
- **Primary tools:** `cp`, `fsck -f`, `fsck -y`.
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

### 40A. Preserve the Baseline and Repair the Copy

```bash
cp ~/testdisk.img ~/testdisk-broken.img
sudo fsck -f ~/testdisk-broken.img
sudo fsck -y ~/testdisk-broken.img
sudo fsck -f ~/testdisk-broken.img
```

The original image was preserved. The copy was checked, processed with automatic confirmation in the controlled lab, and checked again to verify its final state.

> The supplied commands do not themselves introduce corruption. They demonstrate the repair workflow on a copy. A report should not claim that corruption was repaired unless `fsck` output actually recorded inconsistencies and corrections.

<img width="1366" height="662" alt="Fig19_CLI_FilesystemRepair_Copy_Fsck png" src="https://github.com/user-attachments/assets/de8048c5-04fc-4cdd-a05c-9398253ed93f" />


---

## Results and Findings

The professional pattern is preserve, assess, repair the copy, and verify. The final check provides evidence of the resulting filesystem state, while the untouched original supports recovery and comparison.

---

## Challenges and Solutions

| Challenge | Resolution |
|---|---|
| The copy may already have been clean. | The result was described according to the actual `fsck` output rather than assuming corruption that the commands did not create. |

---

## Screenshot Reference
| Figure | Filename | Lab |
|---|---|---|
| Fig 19 | `Fig19_CLI_FilesystemRepair_Copy_Fsck.png` | 40 |
---

## Recommendations

- Image or copy a target before repair.
- Record tool output and hashes where formal evidence is involved.
- Use `-y` only when automatic approval is appropriate.

---

## Conclusion

Lab 40 developed practical competency in fixing filesystems. The work combined controlled execution, verification, and professional documentation, supporting future Linux administration, incident-response, and digital-forensics activities.

---

## Safety and Evidence Handling

All activities were completed in an authorised virtual laboratory. Potentially disruptive operations were limited to test data, loopback images, or controlled system administration tasks. Commands and screenshots were retained to support reproducibility.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem Links, Hardware, Boot Process, Packages and Month 2 Assessment.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
