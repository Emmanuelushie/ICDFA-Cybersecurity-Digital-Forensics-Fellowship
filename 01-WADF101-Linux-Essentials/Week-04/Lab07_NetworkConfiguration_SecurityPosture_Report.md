# Lab 7: Network Configuration and Security Posture

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | Linux Essentials — WADF-2026-M01 |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 04 — Month 1 Assessment |
| Lab | Lab 7 — Network Configuration and Security Posture |
| Date of Submission | 16/07/2026 |

---

## Objectives

- Inspect network interfaces, IP addressing, and routing without changing the system configuration.
- Explain the relationship between network interfaces, IP addresses, gateways, and default routes.
- Verify local hostname resolution and public DNS resolution.
- Identify listening TCP and UDP services and classify their exposure levels.
- Review sudo privileges, SSH configuration, firewall status, and pending system updates.
- Develop practical hardening recommendations based on the observed security posture.

---

## Tools Used

- **`ip`** — displays network interfaces, IP addresses, routes, gateways, and kernel routing decisions.
- **`getent`** — queries system databases for host, user, and group records.
- **`resolvectl` and `/etc/resolv.conf`** — display DNS resolver settings and configured DNS servers.
- **`ss`** — identifies listening TCP and UDP sockets and, where permitted, their associated processes.
- **`sudo` and `id`** — inspect administrative privileges, user identity, and group memberships.
- **`grep`** — extracts selected SSH configuration directives.
- **`ufw` and `nft`** — inspect host-firewall status and rules.
- **`apt`** — installs UFW and reports packages with available updates.

---

## Methodology

### 7A. Interface, Address and Route Investigation

A read-only investigation was conducted to establish the virtual machine's network baseline. No commands were used to modify interfaces, IP addresses, routes, or network services.

```bash
ip -br link
```

Listed all network interfaces in brief format, including each interface name, operational state, and MAC address. This helped identify the active network interface.

```bash
ip -br addr
```

Displayed the IPv4 and IPv6 addresses assigned to each network interface.

```bash
ip route
```

Displayed the system routing table and identified the default route, gateway address, and outbound interface.

```bash
ip route get 1.1.1.1
```

Asked the Linux kernel which route, gateway, interface, and source address it would use to reach an external destination. This verified the expected outbound path without modifying the routing table.

```bash
getent hosts localhost
```

Confirmed that `localhost` resolves through the system name database to a loopback address.

```bash
getent hosts "$(hostname)"
```

Checked whether the virtual machine could resolve its own hostname to an IP address.

**Technical explanation:** A network interface is the physical or virtual adapter through which a system sends and receives network traffic. An IP address is the logical address assigned to that interface, allowing the system to communicate on a network. A default route instructs the kernel to forward traffic to a specified gateway when no more specific route exists. Without a valid default route, the system may communicate with local devices but will normally be unable to reach external networks.

**Screenshot:** `Fig01_CLI_InterfaceAddressRoute_IpBrLink_IpRoute.png`

<img width="1366" height="662" alt="Fig01_CLI_InterfaceAddressRoute_IpBrLink_IpRoute png" src="https://github.com/user-attachments/assets/d48c259e-5a2e-4ad5-984a-026697cec9ce" />


---

### 7B. DNS, Name Resolution and Listening Services

The virtual machine's DNS configuration, local host mappings, public name resolution, and listening network services were examined. Each listening service was assessed according to its bind address and potential exposure.

```bash
resolvectl status 2>/dev/null || cat /etc/resolv.conf
```

Displayed the active DNS resolver configuration. If `resolvectl` was unavailable, `/etc/resolv.conf` was displayed as a fallback.

```bash
cat /etc/hosts
```

Reviewed static hostname mappings and confirmed the presence of loopback and local-hostname entries.

```bash
getent ahosts example.com | head
```

Tested public DNS resolution for `example.com` and displayed the first returned address records.

```bash
getent ahosts icdfa.edu.ng | head
```

Tested DNS resolution for the ICDFA domain and recorded the returned address information or any resolution failure accurately.

```bash
ss -tulpn 2>/dev/null || ss -tul
```

Listed listening TCP and UDP sockets. Associated process information was displayed where the account had sufficient permission.

```bash
ss -ltnp 2>/dev/null || ss -ltn
```

Displayed listening TCP sockets and the local address on which each service was bound.

**Exposure interpretation:** Services bound to `127.0.0.1` or `::1` are accessible only from the local machine. Services bound to `0.0.0.0` or `::` listen on all available interfaces and may be reachable from connected networks. An all-interface binding is not automatically insecure, but it creates a broader exposure that must be justified and protected with suitable firewall, authentication, and access-control measures.

**Screenshot:** `Fig02_CLI_DnsAndHostsResolution_Resolvectl_EtcHosts_Getent.png`

<img width="1366" height="662" alt="Fig02_CLI_DnsAndHostsResolution_Resolvectl_EtcHosts_Getent png" src="https://github.com/user-attachments/assets/0ebd752a-088e-4870-a3b4-b4cc66021f87" />

**Screenshot:** `Fig03_CLI_ListeningServicesAndExposure_SsTulpn_SsLtnp.png`

<img width="1366" height="662" alt="Fig03_CLI_ListeningServicesAndExposure_SsTulpn_SsLtnp png" src="https://github.com/user-attachments/assets/079274cb-3979-4afe-b264-fcd9b64d116e" />

---

### 7C. Basic Linux Security Posture

The system's administrative privileges, SSH configuration, firewall status, and update posture were reviewed. SSH settings were not modified, and available package updates were not installed during the assessment. UFW was installed because the command was not initially available on the minimal virtual machine.

```bash
sudo -l
```

Displayed the commands the current account was authorised to run with elevated privileges.

```bash
id
```

Displayed the current user's UID, primary GID, and supplementary group memberships.

```bash
getent group sudo 2>/dev/null || true
```

Displayed the system record for the `sudo` group and identified its listed members.

```bash
grep -E '^(PermitRootLogin|PasswordAuthentication)' /etc/ssh/sshd_config 2>/dev/null || true
```

Inspected two significant SSH directives without modifying the configuration file. `PermitRootLogin` controls direct root access over SSH, while `PasswordAuthentication` controls password-based authentication.

```bash
sudo apt install ufw -y
```

Installed the Uncomplicated Firewall package with administrative privileges.

```bash
sudo ufw status verbose 2>/dev/null || true
```

Displayed the UFW operational state, default policies, and configured rules.

```bash
sudo nft list ruleset 2>/dev/null | head -n 40 || true
```

Displayed the first 40 lines of the nftables ruleset as an additional firewall inspection method.

```bash
apt list --upgradable 2>/dev/null | head -n 25 || true
```

Displayed the first 25 packages with available updates to assess the system's patch-management posture.

**Hardening recommendations:**

- Disable direct root login over SSH by setting `PermitRootLogin no` where SSH access is required.
- Prefer key-based SSH authentication and disable password authentication after verified key access has been configured.
- Enable a host firewall with a default-deny inbound policy and permit only explicitly required services.
- Apply tested security and maintenance updates regularly to reduce exposure to known vulnerabilities.
- Limit sudo access according to operational responsibilities rather than granting unrestricted administrative privileges.
- Review every service bound to all interfaces and disable or restrict services that are not required.

**Screenshot:** `Fig04_CLI_SudoAndSshPosture_SudoL_GrepSshdConfig.png`

<img width="1366" height="662" alt="Fig04_CLI_SudoAndSshPosture_SudoL_GrepSshdConfig png" src="https://github.com/user-attachments/assets/764d2266-a610-4b2a-94dd-c92aa4865652" />


**Screenshot:** `Fig05_CLI_SudoGroupMembership_GetentGroupSudo.png`

<img width="1366" height="662" alt="Fig05_CLI_SudoGroupMembership_GetentGroupSudo png" src="https://github.com/user-attachments/assets/b7754fab-7020-4439-8e20-582c72afd61c" />

**Screenshot:** `Fig06_CLI_FirewallInstall_AptInstallUfw.png`

<img width="1366" height="662" alt="Fig06_CLI_FirewallInstall_AptInstallUfw png" src="https://github.com/user-attachments/assets/6f194486-f6bf-4e05-8e2e-ba9f6143566c" />

**Screenshot:** `Fig07_CLI_FirewallStatus_UfwStatusVerbose.png`

<img width="1366" height="662" alt="Fig07_CLI_FirewallStatus_UfwStatusVerbose png" src="https://github.com/user-attachments/assets/5a31a5c5-68df-448f-8986-a632428a9a25" />

**Screenshot:** `Fig08_CLI_UpdatablePackages_AptListUpgradable.png`

<img width="1366" height="662" alt="Fig08_CLI_UpdatablePackages_AptListUpgradable png" src="https://github.com/user-attachments/assets/1fa435ff-c3d4-4dd5-983f-318f294044c5" />

---

## Analysis and Findings

The investigation established a structured network baseline covering active interfaces, assigned IP addresses, the default route, DNS configuration, hostname resolution, and listening services. The route inspection demonstrated how the Linux kernel selects an interface, gateway, and source address for external traffic. The name-resolution tests established whether the virtual machine could resolve both local and public hostnames.

The listening-service review demonstrated the security importance of bind addresses. Services restricted to loopback addresses have lower network exposure because remote systems cannot connect to them directly. Services bound to all interfaces have a wider potential attack surface and require deliberate firewall, authentication, and access-control decisions.

The privilege review showed that the laboratory account had broad sudo capabilities. Such access is practical in a controlled training environment, but production systems should apply least privilege and role-based administration. The firewall and package-update checks also demonstrated that system security depends on both network controls and consistent patch management.

SSH exposure should be assessed by comparing configuration directives with the active listening-socket information. Configuration values alone do not establish that the SSH service is running, listening, or reachable from another system.

---

## Challenges and Solutions

| Challenge | Solution |
|---|---|
| The minimal Kali Linux installation did not initially provide UFW. | Installed UFW with `sudo apt install ufw -y`, then inspected its status before applying any firewall-policy changes. |
| Some process names were unavailable in `ss` output without sufficient privilege. | Used the available socket, port, protocol, and bind-address information without inventing unavailable process details. |
| Selected SSH directives did not necessarily appear because they could be commented out or defined in included files. | Treated an empty `grep` result as inconclusive and recommended checking the effective SSH configuration before making a production decision. |
| Firewall posture could appear differently through UFW and nftables. | Reviewed both UFW status and the nftables ruleset instead of relying on only one firewall-management interface. |

---

## Conclusion

Lab 7 demonstrated how to assess Linux network configuration and security posture using controlled and primarily read-only commands. The exercise connected interfaces, addressing, routing, DNS, listening sockets, administrative privileges, SSH configuration, firewall controls, and package updates into one structured baseline assessment.

The resulting findings provide a defensible starting point for system hardening without weakening existing controls during the review. The lab also reinforced the importance of validating a security conclusion with multiple sources of system evidence rather than relying on a single command or configuration file.

---

## Screenshots Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 1 | `Fig01_CLI_InterfaceAddressRoute_IpBrLink_IpRoute.png` | 7A |
| Fig 2 | `Fig02_CLI_DnsAndHostsResolution_Resolvectl_EtcHosts_Getent.png` | 7B |
| Fig 3 | `Fig03_CLI_ListeningServicesAndExposure_SsTulpn_SsLtnp.png` | 7B |
| Fig 4 | `Fig04_CLI_SudoAndSshPosture_SudoL_GrepSshdConfig.png` | 7C |
| Fig 5 | `Fig05_CLI_SudoGroupMembership_GetentGroupSudo.png` | 7C |
| Fig 6 | `Fig06_CLI_FirewallInstall_AptInstallUfw.png` | 7C |
| Fig 7 | `Fig07_CLI_FirewallStatus_UfwStatusVerbose.png` | 7C |
| Fig 8 | `Fig08_CLI_UpdatablePackages_AptListUpgradable.png` | 7C |

*Place each screenshot inside the `Screenshots/` directory using the exact filename shown above. The image links in the Methodology section will then resolve automatically.*

---

## Recommendations

- Preserve the original network state before making configuration changes.
- Record the exact active interface, IP address, gateway, DNS server, and listening ports in the screenshot evidence.
- Compare SSH configuration with active listening sockets before concluding that SSH is exposed.
- Enable firewall rules only after identifying the services that are legitimately required.
- Apply security updates after testing and creating a recovery point.
- Restrict administrative access according to least-privilege principles.
- Repeat the baseline assessment after significant network, firewall, SSH, or package changes.

---

## References

- ICDFA. (2026). *WADF-2026-M01 Student Laboratory Workbook — Week 04: Network Configuration, Linux Security, User Administration and Capstone Challenges.* International Cybersecurity and Digital Forensics Academy.
- Linux man-pages Project. (n.d.). *Linux manual pages: ip(8), ss(8), getent(1), sudo(8), and grep(1).* https://man7.org/linux/man-pages/
- ICDFA. (2026). *WADF 101 Module 1 to Module 6.*
