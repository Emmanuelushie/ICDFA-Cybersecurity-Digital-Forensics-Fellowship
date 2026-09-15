# Lab 37: Runlevels and systemd Targets

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 08 |
| Lab | Lab 37: Runlevels and systemd Targets |
| Date of Submission | 14/08/2026 |


---

## Executive Summary

This lab mapped legacy runlevel concepts to modern systemd targets and inspected the default target, active targets, compatibility runlevel, and target dependencies.

---

## Objectives

- Identify the default systemd target.
- List active targets.
- Relate legacy runlevels to modern targets.
- Inspect target dependencies.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Week 08 laboratory environment.
- **Primary tools:** `systemctl`, `runlevel`, `head`.
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

### 37A. Inspect Targets and Compatibility State

```bash
systemctl get-default
systemctl list-units --type=target
runlevel
systemctl list-dependencies multi-user.target | head -n 20
```

The default and currently active targets were reviewed. `runlevel` supplied a compatibility representation, while the dependency listing showed services and targets required by `multi-user.target`.

<img width="1366" height="662" alt="Fig11_CLI_SystemdTargets_Default_Active png" src="https://github.com/user-attachments/assets/e1906e48-50c2-48cf-992c-ed2b3009e96c" />


<img width="1366" height="662" alt="Fig12_CLI_LegacyRunlevel_Compatibility png" src="https://github.com/user-attachments/assets/05f319da-2f26-4e54-9124-4e00bc9eca5c" />

<img width="1366" height="662" alt="Fig13_CLI_MultiUserTarget_Dependencies png" src="https://github.com/user-attachments/assets/32ec32e1-90b9-491c-8244-ddc7cb6a39c7" />


---

## Results and Findings

Systemd targets group units and express dependencies more flexibly than the traditional numbered runlevel model. Compatibility runlevel output is useful, but systemd target state is the authoritative modern view.

---

## Challenges and Solutions

| Challenge | Resolution |
|---|---|
| Legacy and modern terminology appeared inconsistent. | The legacy values were treated as compatibility labels and mapped to their approximate systemd target equivalents. |

---

## Screenshot Reference
| Figure | Filename | Lab |
|---|---|---|
| Fig 11 | `Fig11_CLI_SystemdTargets_Default_Active.png` | 37 |
| Fig 12 | `Fig12_CLI_LegacyRunlevel_Compatibility.png` | 37 |
| Fig 13 | `Fig13_CLI_MultiUserTarget_Dependencies.png` | 37 |
---

## Recommendations

- Use `systemctl get-default` for the configured boot target.
- Inspect dependencies before changing targets.
- Avoid switching targets on production systems without impact assessment.

---

## Conclusion

Lab 37 developed practical competency in runlevels and systemd targets. The work combined controlled execution, verification, and professional documentation, supporting future Linux administration, incident-response, and digital-forensics activities.

---

## Safety and Evidence Handling

All activities were completed in an authorised virtual laboratory. Potentially disruptive operations were limited to test data, loopback images, or controlled system administration tasks. Commands and screenshots were retained to support reproducibility.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem Links, Hardware, Boot Process, Packages and Month 2 Assessment.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
