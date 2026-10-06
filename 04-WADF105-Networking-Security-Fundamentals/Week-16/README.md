# WADF105 Network Security Fundamentals
## Practical Laboratory 1 and Laboratory 2 Assignment

**Programme:** ICDFA Fellowship in Cybersecurity and Digital Forensics (WADF-2026)  
**Institution:** International Cybersecurity and Digital Forensics Academy (ICDFA)

| Field | Detail |
| --- | --- |
| **Student** | Emmanuel Adie Ushie |
| **Student ID** | C11/26/ICDF/17173 |
| **Instructor** | Mr. Aminu Idris |
| **Course** | WADF105 Network Security Fundamentals |
| **Environment** | Oracle VirtualBox, authorised ICDFA laboratory only |

---

## 1. Overview

This repository contains my submissions for **Practical Laboratory 1** and **Practical Laboratory 2** of WADF105. The two laboratories build on each other and use the same two virtual machines:

- **Laboratory 1** commissions a protected two-VM network (an OPNsense firewall and an Ubuntu client) and verifies addressing, routing, DNS and internet access.
- **Laboratory 2** uses that network to create controlled firewall rules, read firewall logs, correlate decisions with packet captures, inspect states and NAT, and then restore the laboratory.

Each laboratory has its own full report. This README is the entry point.

| Laboratory | Title | Report |
| --- | --- | --- |
| 1 | Two VM Network Commissioning and Connectivity Verification | [Lab01_Report.md](Lab01_Report.md) |
| 2 | OPNsense Firewall Policy Testing, Logging and Packet Analysis | [Lab02_Report.md](Lab02_Report.md) |

## 2. Network Topology

```
                         Internet
                             |
                    VirtualBox NAT (10.0.3.0/24)
                             |
                     [ WAN  em1  10.0.3.15/24 ]
              +--------------------------------------+
              |   icdfa-nslab-firewall-v1 (OPNsense) |
              +--------------------------------------+
                     [ LAN  em0  10.10.10.1/24 ]
                             |
                  Internal Network "ICDFA-LAN"
                             |
                     [ enp0s3  10.10.10.77/24 ]
              +--------------------------------------+
              |    icdfa-nslab-client-v1 (Ubuntu)    |
              |    Default gateway: 10.10.10.1       |
              +--------------------------------------+
```

| Component | VirtualBox connection | Addressing |
| --- | --- | --- |
| Firewall WAN (em1) | NAT | DHCP, 10.0.3.15/24 |
| Firewall LAN (em0) | Internal Network `ICDFA-LAN` | 10.10.10.1/24 |
| Ubuntu client (enp0s3) | Internal Network `ICDFA-LAN` | DHCP, 10.10.10.77/24 |

---

## 3. Laboratory 1 Summary

**Goal:** commission the network and prove it works layer by layer.

| Test | Command | Result |
| --- | --- | --- |
| Gateway reachability | `ping -c 4 10.10.10.1` | Passed, 0% packet loss |
| Internet by IP address | `ping -c 4 1.1.1.1` | Passed, 0% packet loss |
| DNS resolution | `getent hosts opnsense.org` | Passed, address returned |
| Web access | `curl -I https://opnsense.org` | Passed, `HTTP/1.1 200 OK` |
| Packet analysis | Wireshark `arp`, `icmp`, `dns` | ARP, ICMP and DNS traffic captured and identified |

**Issue found and fixed:** the firewall had a WAN address but no usable default route, because a pre-configured upstream gateway (`VROUTER_GW`, 172.16.100.1) was unreachable. Making `WAN_DHCP` (10.0.3.2) the upstream gateway and disabling `VROUTER_GW` restored internet access.

Full detail: [Lab01_Report.md](Lab01_Report.md)

## 4. Laboratory 2 Summary

**Goal:** control traffic with narrow firewall rules and prove the effect with logs and packets.

| Rule | Action | Scope |
| --- | --- | --- |
| `LAB2 BLOCK ICMP TO 1.1.1.1` | Block | ICMP from LAN net to 1.1.1.1/32 |
| `LAB2 BLOCK OUTBOUND HTTP` | Block | TCP from LAN net to any destination, port 80 |

| Check | Result |
| --- | --- |
| Ping to 1.1.1.1 with ICMP rule active | 100% packet loss, while DNS and HTTPS still worked |
| HTTP with HTTP rule active | Failed or timed out, while HTTPS returned headers |
| OPNsense Live View | Blocked entries identified by the LAB2 rule labels |
| Wireshark | No ICMP replies, repeated SYNs on port 80, completed handshake and TLS on port 443 |
| State table | Permitted HTTPS connection with its NAT-translated WAN-side state |
| Restore | Both rules disabled, ping, HTTP and HTTPS working again |

**Issue found and fixed:** the ICMP rule stopped matching after the second rule was added, because it was no longer above the default allow rule. Reordering the rules so both LAB2 rules sat above the defaults corrected it.

Full detail: [Lab02_Report.md](Lab02_Report.md)

---

## 5. Repository Structure

```
.
├── README.md                      # This file
├── Lab01_Report.md                # Laboratory 1 report
├── Lab02_Report.md                # Laboratory 2 report
└── screenshots/
    ├── lab01/
    │   ├── E1_virtualbox_firewall_network.png
    │   ├── E2_virtualbox_client_network.png
    │   ├── E3_opnsense_dashboard.png
    │   ├── E4_ubuntu_ip_and_route.png
    │   ├── E5_connectivity_tests.png
    │   ├── E6a_wireshark_arp.png
    │   ├── E6b_wireshark_icmp.png
    │   ├── E6c_wireshark_dns.png
    │   ├── T1_initial_failed_tests.png
    │   ├── T2_firewall_routing_table.png
    │   └── T3_gateway_configuration_fixed.png
    └── lab02/
        ├── E1_baseline_tests.png
        ├── E2_lan_rules_order.png
        ├── E3_icmp_blocked_dns_https_ok.png
        ├── E4_http_blocked_https_ok.png
        ├── E5_live_view_blocks.png
        ├── E6a_wireshark_icmp.png
        ├── E6b_wireshark_http_syn.png
        ├── E6c_wireshark_https_443.png
        ├── E7_state_table_https.png
        ├── E7b_outbound_nat_mode.png
        ├── E8a_rules_disabled.png
        ├── E8b_restored_tests.png
        ├── T1_icmp_passed_rule_order.png
        └── T2_live_view_pass_entry.png
```

## 6. Evidence Index

### Laboratory 1

| ID | Evidence | File |
| --- | --- | --- |
| E1 | VirtualBox network settings, OPNsense | `screenshots/lab01/E1_virtualbox_firewall_network.png` |
| E2 | VirtualBox network settings, Ubuntu | `screenshots/lab01/E2_virtualbox_client_network.png` |
| E3 | OPNsense console and dashboard, WAN and LAN | `screenshots/lab01/E3_opnsense_dashboard.png` |
| E4 | `ip -4 -br address` and `ip route` | `screenshots/lab01/E4_ubuntu_ip_and_route.png` |
| E5 | Gateway, internet and DNS tests | `screenshots/lab01/E5_connectivity_tests.png` |
| E6 | Wireshark ARP, ICMP and DNS | `screenshots/lab01/E6a_...`, `E6b_...`, `E6c_...` |
| E7 | Default gateway explanation | Section 8 of `Lab01_Report.md` |

### Laboratory 2

| ID | Evidence | File |
| --- | --- | --- |
| E1 | Baseline ping, DNS, HTTP and HTTPS | `screenshots/lab02/E1_baseline_tests.png` |
| E2 | LAN rules with both LAB2 rules above the default allow | `screenshots/lab02/E2_lan_rules_order.png` |
| E3 | Failed ICMP with working DNS and HTTPS | `screenshots/lab02/E3_icmp_blocked_dns_https_ok.png` |
| E4 | Failed HTTP and working HTTPS | `screenshots/lab02/E4_http_blocked_https_ok.png` |
| E5 | Live View block entries | `screenshots/lab02/E5_live_view_blocks.png` |
| E6 | Wireshark: blocked ICMP and HTTP, permitted HTTPS | `screenshots/lab02/E6a_...`, `E6b_...`, `E6c_...` |
| E7 | State table and NAT mode | `screenshots/lab02/E7_...`, `E7b_...` |
| E8 | Rules disabled and connectivity restored | `screenshots/lab02/E8a_...`, `E8b_...` |

---

## 7. Skills Demonstrated

- Configuring a two-VM network in VirtualBox with NAT and an internal network.
- Verifying addressing, default routes, DNS and internet access in layers.
- Diagnosing routing faults from console output, routing tables and gateway status.
- Creating narrow, correctly ordered OPNsense firewall rules and testing their effect.
- Reading firewall logs to identify the rule behind a decision.
- Capturing and interpreting ARP, ICMP, DNS and TCP traffic in Wireshark.
- Explaining states and outbound NAT.
- Restoring a laboratory to a known-good state.

## 8. Tools and Environment

- Oracle VirtualBox
- OPNsense 26.7 (amd64)
- Ubuntu Desktop (client)
- Wireshark
- Command-line tools: `ip`, `resolvectl`, `ping`, `getent`, `curl`, `netstat`

## 9. Safety and Ethics Statement

All work was carried out inside the authorised ICDFA laboratory environment on isolated virtual machines. Firewall rules were created only on the authorised laboratory firewall, were temporary and narrowly scoped, and were disabled at the end of Laboratory 2. The self-signed certificate warning for `https://10.10.10.1` was accepted only for the local laboratory firewall. No traffic was tested or captured outside the laboratory scope.

---

*International Cybersecurity and Digital Forensics Academy | Student Laboratory Portfolio | WADF105*

