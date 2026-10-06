## Two VM Network Commissioning and Connectivity Verification

| Field | Detail |
| --- | --- |
| **Course** | WADF105 Network Security Fundamentals |
| **Programme** | ICDFA Fellowship in Cybersecurity and Digital Forensics (WADF-2026) |
| **Student** | Emmanuel Adie Ushie |
| **Student ID** | C11/26/ICDF/17173 |
| **Instructor** | Mr. Aminu Idris |
| **Date** | 5 October 2026 |
| **Required VMs** | icdfa-nslab-firewall-v1 and icdfa-nslab-client-v1 |

---

## Table of Contents

1. [Objective](#1-objective)
2. [Laboratory Environment and Topology](#2-laboratory-environment-and-topology)
3. [Part A: Prepare VirtualBox (E1, E2)](#3-part-a-prepare-virtualbox)
4. [Part B: Verify OPNsense Interfaces (E3)](#4-part-b-verify-opnsense-interfaces)
5. [Part C: Verify Ubuntu Addressing (E4)](#5-part-c-verify-ubuntu-addressing)
6. [Part D: Connectivity Tests (E5)](#6-part-d-connectivity-tests)
7. [Part E: Packet Analysis in Wireshark (E6)](#7-part-e-packet-analysis-in-wireshark)
8. [Default Gateway Explanation (E7)](#8-default-gateway-explanation-e7)
9. [Troubleshooting Log](#9-troubleshooting-log)
10. [Completion Questions](#10-completion-questions)
11. [Conclusion](#11-conclusion)
12. [Evidence Checklist](#12-evidence-checklist)

---

## 1. Objective

The purpose of this laboratory was to build and verify a small protected network using two virtual machines in VirtualBox, an OPNsense firewall and an Ubuntu client. The learning objectives were to:

- Explain the purpose of WAN and LAN interfaces on a firewall.
- Configure a two-VM VirtualBox network without relying on fixed MAC addresses.
- Obtain or assign a valid IPv4 address, default gateway and DNS server.
- Verify local connectivity, routing, name resolution and internet access.
- Capture and identify ARP, ICMP and DNS packets in Wireshark.

## 2. Laboratory Environment and Topology

### 2.1 Virtual machines

| VM | Role | Operating system |
| --- | --- | --- |
| icdfa-nslab-firewall-v1 | Firewall and gateway | OPNsense 26.7 (amd64) |
| icdfa-nslab-client-v1 | Student workstation and packet-analysis host | Ubuntu Desktop |

### 2.2 Network design

| Component | VirtualBox connection | Addressing | Purpose |
| --- | --- | --- | --- |
| Firewall WAN (em1) | NAT | DHCP, observed 10.0.3.15/24 | Internet-facing interface |
| Firewall LAN (em0) | Internal Network `ICDFA-LAN` | 10.10.10.1/24 | Protected network and default gateway |
| Ubuntu client (enp0s3) | Internal Network `ICDFA-LAN` | DHCP, observed 10.10.10.77/24 | Student workstation |

```
Internet --- VirtualBox NAT --- [WAN em1] OPNsense [LAN em0] --- ICDFA-LAN --- [enp0s3] Ubuntu
                                10.0.3.15          10.10.10.1                  10.10.10.77
```

> **Note:** The lab sheet gives 10.0.2.15/24 as a typical WAN address. VirtualBox assigned 10.0.3.15/24 in my environment. This is a normal variation because the NAT subnet is chosen by VirtualBox, and it does not affect the laboratory outcome.

---

## 3. Part A: Prepare VirtualBox

Both virtual machines were shut down completely before any adapter changes were made, because adapters must not be modified while a VM is running or saved.

**icdfa-nslab-firewall-v1**

| Adapter | Attached to | Notes |
| --- | --- | --- |
| Adapter 1 | [EDIT: NAT or Internal Network, match your final setup] | Cable Connected ticked |
| Adapter 2 | [EDIT: Internal Network `ICDFA-LAN` or NAT, match your final setup] | Cable Connected ticked |
| Adapter 3 / 4 | Disabled | Unused |

**icdfa-nslab-client-v1**

| Adapter | Attached to | Notes |
| --- | --- | --- |
| Adapter 1 | Internal Network `ICDFA-LAN` | Cable Connected ticked |
| Adapter 2 to 4 | Disabled | Unused |

The internal network name `ICDFA-LAN` is case-sensitive and was entered identically on both machines. Bridged Adapter was not used.

### Evidence E1: VirtualBox network settings, OPNsense

<img width="959" height="532" alt="E1_virtualbox_firewall_network png" src="https://github.com/user-attachments/assets/a220c439-b44f-4f92-bd7d-444caf579567" />

*Figure E1: The OPNsense VM has one adapter on NAT for internet access and one adapter on the internal network ICDFA-LAN for the protected LAN.*

### Evidence E2: VirtualBox network settings, Ubuntu

<img width="959" height="500" alt="E2_virtualbox_client_network png" src="https://github.com/user-attachments/assets/a38136c8-3694-4401-8951-c1b3d80922a8" />

*Figure E2: The Ubuntu client has a single adapter attached to the internal network ICDFA-LAN.*

**Why this works:** VirtualBox NAT gives the firewall a path to the internet, while the internal network `ICDFA-LAN` behaves like an isolated Ethernet switch connecting the firewall LAN to the Ubuntu client.

---

## 4. Part B: Verify OPNsense Interfaces

The firewall was started first, and the OPNsense console menu was allowed to finish loading before the Ubuntu client was started.

| Interface | Device | Address |
| --- | --- | --- |
| LAN | em0 | 10.10.10.1/24 |
| WAN | em1 | DHCP, 10.0.3.15/24 |

The LAN address was already configured as 10.10.10.1/24, so it was left unchanged. No gateway was configured on the LAN interface, and the WAN obtained its address automatically through DHCP.

### Evidence E3: OPNsense console and dashboard

<img width="765" height="431" alt="E3_opnsense_dashboard png" src="https://github.com/user-attachments/assets/28ce6401-8acd-4d60-8816-a1e0578c940b" />

*Figure E3: The OPNsense console and web dashboard show the LAN at 10.10.10.1/24 and the WAN with a DHCP-assigned address.*

I accessed the management interface at `https://10.10.10.1` from the Ubuntu client browser and accepted the self-signed certificate warning only for this authorised local firewall.

---

## 5. Part C: Verify Ubuntu Addressing

Commands run on icdfa-nslab-client-v1:

```bash
ip -4 -br address
ip route
resolvectl status
```

| Item | Observed value |
| --- | --- |
| Active interface | enp0s3 |
| IPv4 address | 10.10.10.77/24 |
| Default route | via 10.10.10.1 dev enp0s3 (DHCP) |
| Connected network | 10.10.10.0/24 |

The address lies inside 10.10.10.0/24 and is not 10.10.10.1, and the default route points to the firewall LAN address as required.

### Evidence E4: Ubuntu addressing and routing

<img width="849" height="257" alt="E4_ubuntu_ip_and_route png" src="https://github.com/user-attachments/assets/3c8c5730-27fc-41e8-8f14-2befac9eb54c" />


*Figure E4: Ubuntu holds 10.10.10.77/24 on enp0s3 and its default route points to 10.10.10.1.*

---

## 6. Part D: Connectivity Tests

The tests were run in sequence, because each one verifies a different network layer.

| Test | Command | Expected result | Observed result | What it proves |
| --- | --- | --- | --- | --- |
| 1 | `ping -c 4 10.10.10.1` | Replies received | 4/4 replies, 0% loss, average about 1.8 ms | Ubuntu reaches the firewall LAN interface |
| 2 | `ping -c 4 1.1.1.1` | Replies received | 4/4 replies, 0% loss, average about 43.6 ms | Routing and outbound NAT work |
| 3 | `getent hosts opnsense.org` | Address returned | Address returned (2001:1af8:2050:a001:1::1) | DNS name resolution works |
| 4 | `curl -I https://opnsense.org` | HTTP response headers | `HTTP/1.1 200 OK` | TCP, TLS and web connectivity work |

### Evidence E5: Successful connectivity tests

<img width="1863" height="833" alt="E5_connectivity_tests png" src="https://github.com/user-attachments/assets/fcd4c2c0-e22e-49b8-9ced-d109bee3a3ad" />


*Figure E5: All four tests succeeded, confirming gateway reachability, internet routing, DNS resolution and HTTPS access.*

> These results were obtained after correcting a gateway fault on the firewall. The initial failures and the fix are described in [Section 9](#9-troubleshooting-log).

---

## 7. Part E: Packet Analysis in Wireshark

Wireshark was started on the interface `enp0s3`. While the capture was running, the following commands were repeated in a second terminal:

```bash
ping -c 4 10.10.10.1
getent hosts opnsense.org
```

The capture was then stopped and each display filter was applied in turn.

### 7.1 ARP (filter: `arp`)

| Packet | Type | Information |
| --- | --- | --- |
| 12 | Request | Who has 10.10.10.1? Tell 10.10.10.77 |
| 13 | Reply | 10.10.10.1 is at 08:00:27:c5:e8:2b |

The Ubuntu client (MAC 08:00:27:da:be:46) broadcast an ARP request asking for the owner of 10.10.10.1, and the firewall replied with its MAC address 08:00:27:c5:e8:2b. A second request and reply followed later in the capture (packets 16 and 17), consistent with the ARP cache entry being refreshed.

<img width="1858" height="969" alt="E6a_wireshark_arp png" src="https://github.com/user-attachments/assets/a56c16d3-9a6f-46a1-985e-a6334dee5df5" />


*Figure E6a: The `arp` filter shows the client asking who has 10.10.10.1 and the firewall replying with its MAC address 08:00:27:c5:e8:2b.*

### 7.2 ICMP (filter: `icmp`)

Four echo requests were sent from 10.10.10.77 to 10.10.10.1, and four echo replies were returned by the OPNsense firewall.

<img width="1858" height="943" alt="E6b_wireshark_icmp png" src="https://github.com/user-attachments/assets/d9b790d2-9847-458a-be44-8a45b487db31" />


*Figure E6b: The `icmp` filter shows four echo requests from Ubuntu and four matching echo replies from OPNsense.*

### 7.3 DNS (filter: `dns`)

The client sent a DNS query for `opnsense.org` and received a corresponding response. [EDIT: add the DNS server address and record type seen in your capture, for example the query type A or AAAA and the server IP.]

<img width="1860" height="973" alt="E6c_wireshark_dns png" src="https://github.com/user-attachments/assets/cabf758e-1e99-48f3-a7b5-d47295ecca13" />


*Figure E6c: The `dns` filter shows the query for opnsense.org and the response that returned its address.*

---

## 8. Default Gateway Explanation (E7)

The Ubuntu client uses 10.10.10.1 as its default gateway because that address belongs to the OPNsense firewall LAN interface, which is the only router on the 10.10.10.0/24 network. Ubuntu can reach other hosts in its own subnet directly, but any destination outside 10.10.10.0/24, such as 1.1.1.1 or opnsense.org, must be handed to a router for forwarding. The firewall receives that traffic on its LAN interface, applies its rules, translates the source address through outbound NAT, and forwards it out of the WAN interface toward the internet. The default route therefore tells Ubuntu where to send every packet that has no more specific route.

---

## 9. Troubleshooting Log

### 9.1 Symptoms

When the Ubuntu client was first tested, the LAN worked but nothing beyond the firewall did.

| Test | Result |
| --- | --- |
| `ping 10.10.10.1` | Passed |
| `ping 1.1.1.1` | Failed: "Destination Host Unreachable" from 10.10.10.1 |
| `getent hosts opnsense.org` | Failed: no result |
| `curl` to opnsense.org | Failed: "Could not resolve host" |

![T1 Initial failed connectivity tests](screenshots/lab01/T1_initial_failed_tests.png)

*Figure T1: The initial tests show the gateway answering but the internet and DNS tests failing.*

### 9.2 Diagnosis

1. The "Destination Host Unreachable" message came from the firewall itself (10.10.10.1), which showed that Ubuntu was working and that the firewall had no usable route to the internet.
2. The OPNsense console initially showed the WAN interface (em1) without an IPv4 address. [EDIT: describe what you changed so that the WAN received 10.0.3.15, for example correcting the adapter assignment or reconnecting the cable.] The WAN then obtained 10.0.3.15/24 by DHCP.
3. A ping to 1.1.1.1 from the firewall console (option 7) still failed with "No route to host".
4. In the firewall shell, `netstat -rn -f inet` showed **no default IPv4 route**. A single host route to 192.168.0.1 via 10.0.3.2 appeared, which is outside the 10.0.3.0/24 WAN subnet and could not serve as a default route.

![T2 Firewall routing table without default route](screenshots/lab01/T2_firewall_routing_table.png)

*Figure T2: The firewall IPv4 routing table has no default route, which explains the "No route to host" error.*

5. In **System > Gateways > Configuration**, a pre-configured gateway named `VROUTER_GW` (172.16.100.1) was marked as upstream and active, but showed 100% packet loss and an offline status. The correct gateway, `WAN_DHCP` (10.0.3.2), was online but was not marked as the upstream gateway.

### 9.3 Root cause

A pre-configured upstream gateway from the laboratory image (`VROUTER_GW`, 172.16.100.1) was unreachable in this VirtualBox NAT environment. Because it held the upstream role, OPNsense had no working default route, even though the WAN interface had a valid DHCP address.

### 9.4 Resolution

1. Edited `WAN_DHCP` (10.0.3.2) and ticked **Upstream Gateway**.
2. Edited `VROUTER_GW` and ticked **Disabled**.
3. Clicked **Apply**.
4. [As a temporary measure before the permanent fix, I added a default route in the firewall shell with `route add default 10.0.3.2`."]

![T3 Corrected gateway configuration](screenshots/lab01/T3_gateway_configuration_fixed.png)

*Figure T3: WAN_DHCP is now the active upstream gateway and VROUTER_GW is disabled.*

### 9.5 Verification

After the change, all four tests in Section 6 passed (Evidence E5). Reaching the internet by IP address and resolving a domain name confirmed that routing, outbound NAT and DNS were all working.

### 9.6 Lessons learned

- Test connectivity in layers: LAN first, then routing by IP address, then DNS, then application traffic.
- The source of an ICMP error message shows which device is reporting the problem. Here the firewall reported it, so the fault was on the firewall and not on the client.
- A device can hold a valid WAN address and still have no default route. Checking the routing table and gateway status is part of verifying a WAN.
- Pre-configured settings in a lab image may not match the host environment and should be reviewed.

---

## 10. Completion Questions

**1. What is the difference between the OPNsense WAN and LAN interfaces?**
The WAN interface faces the untrusted external network (here, the internet through VirtualBox NAT) and receives its address by DHCP. The LAN interface faces the protected internal network, has the static address 10.10.10.1/24, acts as the gateway for internal clients and provides DHCP to them. The firewall controls and translates traffic passing between the two.

**2. Why must both internal adapters use the same VirtualBox network name?**
VirtualBox uses the internal network name to decide which adapters share the same virtual switch. Adapters with exactly the same name are placed on one isolated Layer 2 network and can exchange frames, while adapters with different names (including a case difference) end up on separate networks and cannot communicate, so DHCP and ping would fail.

**3. What information does the default route provide to Ubuntu?**
The default route tells Ubuntu which next-hop router (10.10.10.1) and interface (enp0s3) to use for any destination that is not in its directly connected network 10.10.10.0/24. Without it, Ubuntu could only communicate with hosts on its own subnet.

**4. Which packet exchange allows Ubuntu to learn the firewall MAC address?**
The Address Resolution Protocol (ARP) exchange. Ubuntu broadcasts an ARP request asking "Who has 10.10.10.1?", and the firewall replies with an ARP reply stating "10.10.10.1 is at 08:00:27:c5:e8:2b".

**5. Why does a successful ping to 1.1.1.1 not automatically prove that DNS is working?**
Pinging an IP address does not involve any name lookup, so it only proves routing and NAT. DNS translates names into IP addresses and is a separate service, so it can fail even when IP connectivity works. This is why a separate test such as `getent hosts opnsense.org` is needed.

---

## 11. Conclusion

The two-VM laboratory network was commissioned and verified successfully. The Ubuntu client obtained 10.10.10.77/24 from the OPNsense DHCP service, reached the firewall at 10.10.10.1, reached the internet by IP address, resolved a domain name and retrieved HTTP headers over TLS. Wireshark captures identified the ARP request and reply, ICMP echo exchange and DNS query and response.

During the laboratory I diagnosed and corrected a routing fault caused by an unreachable pre-configured upstream gateway, which strengthened my understanding of layered troubleshooting, default routes and gateway configuration in OPNsense.

---

## 12. Evidence Checklist

| ID | Evidence | Included |
| --- | --- | --- |
| E1 | VirtualBox network settings, OPNsense (NAT and ICDFA-LAN) 
| E2 | VirtualBox network settings, Ubuntu (ICDFA-LAN)
| E3 | OPNsense console or dashboard showing WAN and LAN 
| E4 | `ip -4 -br address` and `ip route` output
| E5 | Successful gateway, internet and DNS tests
| E6 | Wireshark ARP, ICMP and DNS views with filters visible
| E7 | Default gateway explanation | [x] Section 8 |
| T1 to T3 | Troubleshooting screenshots (optional supporting evidence)

---

*International Cybersecurity and Digital Forensics Academy | Student Laboratory Report | WADF105 Practical Laboratory 1*

