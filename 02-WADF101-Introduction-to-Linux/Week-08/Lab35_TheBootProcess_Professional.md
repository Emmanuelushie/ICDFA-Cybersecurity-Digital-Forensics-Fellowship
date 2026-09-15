# Lab 35: The Boot Process

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 08 |
| Lab | Lab 35: The Boot Process |
| Date of Submission | 14/08/2026 |


---

## Executive Summary

This lab traced the current boot using systemd timing information, service startup rankings, dependency chains, journal records, and kernel ring-buffer messages.

---

## Objectives

- Measure major boot phases.
- Identify slow-starting services.
- Review the critical dependency chain.
- Inspect current-boot and kernel messages.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Week 08 laboratory environment.
- **Primary tools:** `systemd-analyze`, `journalctl`, `dmesg`, `head`, `less`.
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

### 35A. Analyse Boot Timing and Dependencies

```bash
systemd-analyze
systemd-analyze blame
systemd-analyze critical-chain
```

The commands reported overall startup timing, ranked units by activation duration, and displayed the dependency chain affecting boot completion.

### 35B. Review Boot Logs

```bash
journalctl -b | head -n 50
dmesg | less
```

`journalctl -b` restricted results to the current boot. `dmesg` displayed kernel messages associated with early hardware and driver initialisation.

<img width="1366" height="662" alt="Fig05_CLI_BootTiming_SystemdAnalyze_Blame png" src="https://github.com/user-attachments/assets/1d9d6a5c-bc2c-4e50-bcc7-7e4272600c1a" />


<img width="1366" height="662" alt="Fig06_CLI_BootCriticalChain_SystemdAnalyze png" src="https://github.com/user-attachments/assets/54f5b264-f345-45dd-8705-fc6e17b3d22f" />


<img width="1366" height="662" alt="Fig07_CLI_BootLogs_Journalctl_Dmesg png" src="https://github.com/user-attachments/assets/607bcc15-c477-4514-b73d-664748c1eb92" />


---

## Results and Findings

Boot duration was divided among firmware, bootloader, kernel, and userspace phases. A unit appearing high in `blame` is not automatically the sole cause of total delay because units may start in parallel; the critical chain provides dependency context.

---

## Challenges and Solutions

| Challenge | Resolution |
|---|---|
| Boot output was lengthy. | The output was limited with `head` or reviewed interactively with `less` while preserving the full source logs. |

---

## Screenshot Reference
| Figure | Filename | Lab |
|---|---|---|
| Fig 05 | `Fig05_CLI_BootTiming_SystemdAnalyze_Blame png` | 35 |
| Fig 06 | `Fig06_CLI_BootCriticalChain_SystemdAnalyze png` | 35 |
| Fig 07 | `Fig07_CLI_BootLogs_Journalctl_Dmesg png` | 35 |



---

## Recommendations

- Use both `blame` and `critical-chain` before diagnosing slow boot.
- Do not disable services solely because they appear in timing output.
- Preserve current-boot logs before rebooting during an investigation.

---

## Conclusion

Lab 35 developed practical competency in the boot process. The work combined controlled execution, verification, and professional documentation, supporting future Linux administration, incident-response, and digital-forensics activities.

---

## Safety and Evidence Handling

All activities were completed in an authorised virtual laboratory. Potentially disruptive operations were limited to test data, loopback images, or controlled system administration tasks. Commands and screenshots were retained to support reproducibility.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem Links, Hardware, Boot Process, Packages and Month 2 Assessment.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
