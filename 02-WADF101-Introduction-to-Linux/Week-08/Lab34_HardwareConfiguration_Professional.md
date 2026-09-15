# Lab 34: Hardware Configuration

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 08 |
| Lab | Lab 34: Hardware Configuration |
| Date of Submission | 14/08/2026 |


---

## Executive Summary

This lab produced a read-only inventory of the virtual machine hardware presented by VirtualBox, including processor, memory, block devices, USB devices, PCI devices, and firmware-reported system identity.

---

## Objectives

- Collect CPU and memory information.
- Identify block, USB, and PCI devices.
- Review firmware-provided system information.
- Distinguish guest-visible virtual hardware from physical host hardware.

---

## Tools and Environment

- **Kali Linux virtual machine:** Controlled Week 08 laboratory environment.
- **Primary tools:** `lscpu`, `free`, `lsblk`, `lsusb`, `lspci`, `dmidecode`.
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

### 34A. Collect the Hardware Inventory

```bash
lscpu
free -h
lsblk
lsusb
lspci
sudo dmidecode -t system
```

`lscpu` reported processor architecture and topology, while `free -h` displayed current memory and swap information. `lsblk` listed storage devices hierarchically. `lsusb` and `lspci` displayed devices visible to the guest kernel. `dmidecode` read system descriptors supplied by the virtual firmware.

<img width="1366" height="662" alt="Fig03_CLI_HardwareInventory_Lscpu_Free_Lsblk png" src="https://github.com/user-attachments/assets/9113ec14-049a-4aba-ad2a-07bb06352bca" />


<img width="1366" height="662" alt="Fig04_CLI_DeviceInventory_Lsusb_Lspci_Dmidecode png" src="https://github.com/user-attachments/assets/65334f86-c46a-4187-856a-4fd97fddbb78" />


---

## Results and Findings

The inventory represented devices exposed to the guest, not necessarily the host computer’s physical components. The `available` memory value was more useful than a simple free-memory figure because Linux uses unused memory for cache.

---

## Challenges and Solutions

| Challenge | Resolution |
|---|---|
| Some host devices were not visible. | VirtualBox exposes selected virtual hardware to the guest. Results were documented as the VM inventory rather than the host inventory. |

---

## Screenshot Reference

Screenshot filenames in this rewritten report are professional placeholders derived from the embedded evidence in the source document. Rename exported images to match these links, or update the links to the filenames you select.

---

## Recommendations

- Capture inventory output before making configuration changes.
- Record VM settings alongside guest results.
- Use read-only commands for baseline acquisition.

---

## Conclusion

Lab 34 developed practical competency in hardware configuration. The work combined controlled execution, verification, and professional documentation, supporting future Linux administration, incident-response, and digital-forensics activities.

---

## Safety and Evidence Handling

All activities were completed in an authorised virtual laboratory. Potentially disruptive operations were limited to test data, loopback images, or controlled system administration tasks. Commands and screenshots were retained to support reproducibility.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 8 Practical Labs: Labs 33–42, Filesystem Links, Hardware, Boot Process, Packages and Month 2 Assessment.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
