# Lab 42: Managing Shared Libraries

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 08 |
| Lab | Lab 42: Managing Shared Libraries |
| Date of Submission | 14/08/2026 |


---

## Executive Summary

This lab examined how dynamically linked programs resolve shared libraries through embedded requirements, linker configuration, cache entries, and environment variables.

---

## Objectives

- Inspect a program’s shared-library dependencies.
- Review linker configuration and drop-in files.
- Query the linker cache.
- Rebuild the cache in a controlled environment.
- Inspect `LD_LIBRARY_PATH`.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Week 08 laboratory environment.
- **Primary tools:** `ldd`, `ldconfig`, `/etc/ld.so.conf`, `/etc/ld.so.conf.d/`, `LD_LIBRARY_PATH`.
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

### 42A. Inspect Dependencies and Configuration

```bash
ldd "$(command -v bash)"
cat /etc/ld.so.conf
ls /etc/ld.so.conf.d/
ldconfig -p | wc -l
ldconfig -p | grep libc
echo "$LD_LIBRARY_PATH"
```

The commands displayed Bash dependencies, configured search sources, cache entries, libc resolution, and any session-specific search path.

### 42B. Rebuild the Linker Cache

```bash
sudo ldconfig -v 2>&1 | head -n 10
```

The cache was rebuilt and a limited portion of combined output was displayed. This privileged action is normally used after library configuration changes or manual library installation.

<img width="1366" height="662" alt="Fig24_CLI_SharedLibraries_LddBash png" src="https://github.com/user-attachments/assets/dc53b2c1-ca5d-455b-a214-59d694d9eccf" />


<img width="1366" height="662" alt="Fig25_CLI_LinkerConfig_LdconfigCache png" src="https://github.com/user-attachments/assets/a5207584-dc93-4f24-85d2-8962bb136b0e" />

---

## Results and Findings

Dynamic executables rely on runtime library resolution. The linker cache improves resolution efficiency, while configuration files and environment variables influence search paths. `LD_LIBRARY_PATH` can be useful for testing but may create security and reproducibility risks.

---

## Challenges and Solutions

| Challenge | Resolution |
|---|---|
| Verbose `ldconfig` output included diagnostic messages. | Standard error was combined with standard output before limiting the display so the evidence captured both channels. |

---

## Screenshot Reference
| Figure | Filename | Lab |
|---|---|---|
| Fig 24 | `Fig24_CLI_SharedLibraries_LddBash.png` | 42 |
| Fig 25 | `Fig25_CLI_LinkerConfig_LdconfigCache.png` | 42 |

---

## Recommendations

- Avoid untrusted paths in `LD_LIBRARY_PATH`.
- Use package management for libraries where possible.
- Run `ldconfig` only after authorised configuration changes.
- Use `ldd` cautiously on untrusted executables because implementations may involve security risk; prefer safer inspection tools in hostile-analysis contexts.

---

## Conclusion

Lab 42 developed practical competency in managing shared libraries. The work combined controlled execution, verification, and professional documentation, supporting future Linux administration, incident-response, and digital-forensics activities.

---

## Safety and Evidence Handling

All activities were completed in an authorised virtual laboratory. Potentially disruptive operations were limited to test data, loopback images, or controlled system administration tasks. Commands and screenshots were retained to support reproducibility.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem Links, Hardware, Boot Process, Packages and Month 2 Assessment.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
