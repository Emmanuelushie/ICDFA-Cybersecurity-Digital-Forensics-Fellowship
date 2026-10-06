# WADF105 | Practical Laboratory 2 Report
## OPNsense Firewall Policy Testing, Logging and Packet Analysis

| Field | Detail |
| --- | --- |
| **Course** | WADF105 Network Security Fundamentals |
| **Programme** | ICDFA Fellowship in Cybersecurity and Digital Forensics (WADF-2026) |
| **Student** | Emmanuel Adie Ushie |
| **Student ID** | C11/26/ICDF/17173 |
| **Instructor** | Mr. Aminu Idris |
| **Date** | 5 October 2026 |
| **Required VMs** | icdfa-nslab-client-v1 and icdfa-nslab-firewall-v1 (from Laboratory 1) |

---

## Table of Contents

1. [Objective](#1-objective)
2. [Environment and Starting State](#2-environment-and-starting-state)
3. [Part A: Baseline (E1)](#3-part-a-baseline-e1)
4. [Part B: Rule Order Review](#4-part-b-rule-order-review)
5. [Part C: Block ICMP to One Address (E2, E3)](#5-part-c-block-icmp-to-one-address-e2-e3)
6. [Part D: Block Outbound HTTP, Allow HTTPS (E4)](#6-part-d-block-outbound-http-allow-https-e4)
7. [Part E: Firewall Log Analysis (E5)](#7-part-e-firewall-log-analysis-e5)
8. [Part F: Packet Correlation in Wireshark (E6)](#8-part-f-packet-correlation-in-wireshark-e6)
9. [Part G: States and Automatic NAT (E7)](#9-part-g-states-and-automatic-nat-e7)
10. [Part H: Restoring the Laboratory (E8)](#10-part-h-restoring-the-laboratory-e8)
11. [Troubleshooting Log](#11-troubleshooting-log)
12. [Analysis Questions](#12-analysis-questions)
13. [Conclusion](#13-conclusion)
14. [Evidence Checklist](#14-evidence-checklist)

---

## 1. Objective

The objective was to create controlled firewall rules on OPNsense, verify their effect from the Ubuntu client, read the firewall logs to identify which rule made each decision, correlate those decisions with a packet capture, and then restore normal access. The learning objectives were to:

- Explain how top-down rule processing affects traffic.
- Create and test narrow rules without blocking all connectivity.
- Use protocol, source, destination and port fields correctly.
- Identify the rule that blocked a packet from the firewall log.
- Correlate firewall decisions with packet-capture evidence.
- Disable the test rules and confirm normal connectivity returns.

## 2. Environment and Starting State

| Component | Detail |
| --- | --- |
| Firewall | icdfa-nslab-firewall-v1, OPNsense 26.7, LAN (em0) 10.10.10.1/24, WAN (em1) 10.0.3.15/24 via VirtualBox NAT |
| Client | icdfa-nslab-client-v1, Ubuntu, enp0s3 10.10.10.77/24, default gateway 10.10.10.1 |
| Management | OPNsense dashboard at `https://10.10.10.1` from the Ubuntu browser |

The starting state was the commissioned network from Laboratory 1. The firewall was started first, then the Ubuntu client.

---

## 3. Part A: Baseline (E1)

The IPv4 address and routes were recorded first, then the baseline tests were run before any rule was created.

```bash
ip -4 -br address
ip route
ping -c 4 1.1.1.1
getent hosts example.com
curl -I http://example.com
curl -I https://example.com
```

| Check | Result |
| --- | --- |
| IPv4 address | 10.10.10.77/24 on enp0s3 |
| Default route | via 10.10.10.1 |
| `ping -c 4 1.1.1.1` | 4 recieved, 0% packet loss |
| `getent hosts example.com` | 2026:4700:10::ac42:93f3 |
| `curl -I http://example.com` | HTTP/1.1 200 OK |
| `curl -I https://example.com` | HTTP/2 200 |

All baseline tests passed, so policy testing could begin. Any later failure could therefore be attributed to a new rule and not to the original network.

### Evidence E1: Baseline tests

<img width="959" height="521" alt="E1_baseline_tests png" src="https://github.com/user-attachments/assets/669207e1-fc33-4e5f-acac-44f789e413f2" />


*Figure E1: Before any rule was created, ping, DNS, HTTP and HTTPS from icdfa-nslab-client-v1 all succeeded.*

---

## 4. Part B: Rule Order Review

In **Firewall > Rules > LAN**, the existing rules were "Default allow LAN to any rule" (IPv4) and "Default allow LAN IPv6 to any rule". The broad allow rule lets the client reach external destinations.

OPNsense evaluates interface rules from top to bottom and stops at the first match. A specific block rule therefore only takes effect when it is placed **above** the broad allow rule. The default allow rule was not edited or deleted. Both temporary rules were created as new rules with the **+** button.

---

## 5. Part C: Block ICMP to One Address (E2, E3)

### 5.1 Rule configuration

| Field | Value |
| --- | --- |
| Action | Block |
| Quick | Enabled |
| Interface | LAN |
| Direction | In |
| TCP/IP Version | IPv4 |
| Protocol | ICMP |
| ICMP type | Any |
| Source | LAN net |
| Destination | Single host or Network: 1.1.1.1/32 |
| Log | Enabled |
| Description | LAB2 BLOCK ICMP TO 1.1.1.1 |

The rule was saved, moved above "Default allow LAN to any rule", and applied.

### 5.2 Test

```bash
ping -c 4 1.1.1.1
getent hosts example.com
curl -I https://example.com
```

| Test | Result |
| --- | --- |
| `ping -c 4 1.1.1.1` | 100% packet loss |
| `getent hosts example.com` | 2026:4700:10::ac42:93f3 |
| `curl -I https://example.com` | HTTP/2 200 |

A failed ping alongside successful DNS and HTTPS demonstrates **protocol-specific filtering**. The internet connection is not down, only ICMP to the one address is blocked.

### Evidence E2: LAN rules in the correct order

<img width="959" height="495" alt="E2_lan_rules_order png" src="https://github.com/user-attachments/assets/f9f1cb2a-d18a-437f-b102-e3f845579110" />


*Figure E2: Both LAB2 block rules sit above the Default allow LAN to any rule on the LAN interface.*

### Evidence E3: Blocked ICMP, working DNS and HTTPS

<img width="942" height="472" alt="E3_icmp_blocked_dns_https_ok png" src="https://github.com/user-attachments/assets/0f573c02-f181-445b-925a-911a433774f4" />

*Figure E3: Ping to 1.1.1.1 shows 100% packet loss while DNS resolution and HTTPS to example.com still succeed.*

---

## 6. Part D: Block Outbound HTTP, Allow HTTPS (E4)

### 6.1 Rule configuration

| Field | Value |
| --- | --- |
| Action | Block |
| Interface | LAN |
| Direction | In |
| TCP/IP Version | IPv4 |
| Protocol | TCP |
| Source | LAN net |
| Destination | Any |
| Destination port range | HTTP to HTTP (port 80) |
| Log | Enabled |
| Description | LAB2 BLOCK OUTBOUND HTTP |

The rule was saved, positioned above the default allow rule, and applied.

### 6.2 Test

```bash
curl --max-time 10 -I http://example.com
curl --max-time 10 -I https://example.com
```

| Test | Result |
| --- | --- |
| HTTP (port 80) |curl: connection timed out |
| HTTPS (port 443) | Went through |

The `-L` option was not used, so no redirect was followed.

### Evidence E4: HTTP blocked, HTTPS permitted

<img width="959" height="494" alt="E4_http_blocked_https_ok png" src="https://github.com/user-attachments/assets/f0626bee-5375-48ea-8d5b-7004946bc76a" />


*Figure E4: The HTTP request to port 80 fails while the HTTPS request to port 443 returns response headers.*

---

## 7. Part E: Firewall Log Analysis (E5)

In **Firewall > Log Files > Live View**, the block entries were generated again by repeating the ping and the HTTP `curl`, and the view was filtered with the label `LAB2`.

### 7.1 Blocked TCP port 80 entry

| Field | Value |
| --- | --- |
| Timestamp | 2026-10-05T15:14:13 |
| Interface | LAN (em0) |
| Direction | in |
| Source IP and port | 10.10.10.77 : 33304 |
| Destination IP and port | 185.125.190.99 : 80 |
| Protocol | TCP (protocol number 6) |
| TCP flags | S (SYN, a new connection attempt) |
| Action | block |
| Reason | match |
| Rule number | 89 |
| Rule label | LAB2 BLOCK OUTBOUND HTTP |

### 7.2 Blocked ICMP entries

| Field | Value |
| --- | --- |
| Timestamps | 2026-10-05T16:55:44 to 16:55:47 (four entries, one per ping) |
| Interface and direction | LAN, in |
| Source IP | 10.10.10.77 |
| Destination IP | 1.1.1.1 |
| Protocol | ICMP (no port, because ICMP does not use ports) |
| Action | block |
| Rule label | LAB2 BLOCK ICMP TO 1.1.1.1 |

### 7.3 Why the traffic matched the block rules

OPNsense checks rules from the top down and stops at the first match. Each packet arrived inbound on the LAN interface from `10.10.10.77`. The ICMP packets matched `LAB2 BLOCK ICMP TO 1.1.1.1` (ICMP to 1.1.1.1/32), and the TCP SYN packets matched `LAB2 BLOCK OUTBOUND HTTP` (TCP to port 80). Both rules sit above "Default allow LAN to any rule", so they were evaluated first and the packets were blocked before the allow rule was ever reached.

The same HTTP connection attempt appears several times in the log with the same source port because `curl` retransmits its SYN when it receives no answer.

### Evidence E5: Live View block entries

<img width="927" height="449" alt="E5_live_view_blocks png" src="https://github.com/user-attachments/assets/0c0bbdfc-dbb2-44c2-9176-8b7da708208a" />


*Figure E5: Live View filtered on LAB2 shows ICMP packets to 1.1.1.1 and TCP port 80 connection attempts from 10.10.10.77 blocked by the two LAB2 rules.*

---

## 8. Part F: Packet Correlation in Wireshark (E6)

Wireshark was started on the active interface `enp0s3`. While capturing, the following commands were run in a second terminal:

```bash
ping -c 4 1.1.1.1
curl --max-time 10 -I http://example.com
curl --max-time 10 -I https://example.com
```

The capture was stopped and each display filter was applied in turn.

| Display filter | Observation |
| --- | --- |
| `icmp && ip.addr == 1.1.1.1` | Echo requests leave Ubuntu, but no echo replies return. [EDIT: add packet numbers and count] |
| `tcp.dstport == 80` | Repeated TCP SYN packets to port 80 with no SYN-ACK reply, which are retransmissions. [EDIT: add packet numbers and approximate timing] |
| `tcp.port == 443` | A completed three-way handshake (SYN, SYN-ACK, ACK) followed by encrypted TLS traffic. [EDIT: add packet numbers] |

### Correlation with the firewall log

The unanswered echo requests correspond to the `block` entries for `LAB2 BLOCK ICMP TO 1.1.1.1`. The repeated port 80 SYN packets correspond to the `LAB2 BLOCK OUTBOUND HTTP` entries (flag `S`). The firewall **silently drops** blocked packets, so the client receives no reply and keeps retrying. The permitted HTTPS connection completes its handshake because it matches no block rule and is accepted by the default allow rule.

### Evidence E6a: Blocked ICMP

<img width="1859" height="902" alt="E6a_wireshark_icmp png" src="https://github.com/user-attachments/assets/c4973b86-2462-4936-ad44-584c5c4c0f91" />


*Figure E6a: The filter icmp && ip.addr == 1.1.1.1 shows echo requests leaving Ubuntu with no echo replies.*

### Evidence E6b: Blocked HTTP

<img width="1854" height="899" alt="E6b_wireshark_http_syn png" src="https://github.com/user-attachments/assets/999323e0-532d-49da-be1b-03289963a9f9" />


*Figure E6b: The filter tcp.dstport == 80 shows repeated SYN packets to port 80 with no response.*

---

## 9. Part G: States and Automatic NAT (E7)

In **Firewall > Diagnostics > States**, the table was filtered on `10.10.10.77` after generating a fresh HTTPS request.

| Side | Source | Nat | Destination | State | Rule |
| --- | --- | --- | --- | --- | --- |
| LAN side | 10.10.10.77:33166 | none | 34.107.243.93:443 | ESTABLISHED | Default allow LAN to any rule |
| WAN side | 10.0.3.15:25366 | 10.10.10.77:33166 | 34.107.243.93:443 | ESTABLISHED | let out anything from firewall host itself |

The LAN-side state shows the permitted connection. The WAN-side state shows the same connection after outbound NAT: the private source `10.10.10.77` was translated to the firewall WAN address `10.0.3.15` with a different source port, and the Nat column records the original client address and port.

In **Firewall > NAT > Outbound**, the outbound NAT mode was Automatic outbound NAT rule generation. The mode was not changed.

### Evidence E7: State table

<img width="926" height="445" alt="E7_state_table_https png" src="https://github.com/user-attachments/assets/69eec0fa-395c-4fb9-b6f9-7a220c9d6769" />


*Figure E7: The state table, filtered on 10.10.10.77, shows a permitted HTTPS connection to port 443 and its WAN-side state, where outbound NAT translated the source to 10.0.3.15.*

---

## 10. Part H: Restoring the Laboratory (E8)

1. In **Firewall > Rules > LAN**, both `LAB2 BLOCK ICMP TO 1.1.1.1` and `LAB2 BLOCK OUTBOUND HTTP` were **disabled** (not deleted).
2. The changes were applied.
3. Old states were cleared only where needed.
4. The tests were repeated.

```bash
ping -c 4 1.1.1.1
curl --max-time 10 -I http://example.com
curl --max-time 10 -I https://example.com
```

| Test | Result |
| --- | --- |
| `ping -c 4 1.1.1.1` | 4 received, 0% packet loss |
| `curl --max-time 10 -I http://example.com` | went through successfully |
| `curl --max-time 10 -I https://example.com` | went through successfully |

All three tests worked again, so the laboratory was returned to a known-good state.

### Evidence E8: Rules disabled and connectivity restored

<img width="927" height="414" alt="E8a_rules_disabled png" src="https://github.com/user-attachments/assets/be33e57f-06ce-4cfd-800c-ca042a3d0ae6" />


*Figure E8a: Both LAB2 rules are disabled on the LAN interface and the default allow rules remain active.*

<img width="928" height="356" alt="E8b_restored_tests png" src="https://github.com/user-attachments/assets/6c08461f-aa96-45db-ae35-8bd5d66efb6e" />

*Figure E8b: Ping, HTTP and HTTPS all succeed again after the two test rules were disabled.*

---

## 11. Troubleshooting Log

### 11.1 Symptom

After the second rule (`LAB2 BLOCK OUTBOUND HTTP`) had been created, `ping -c 4 1.1.1.1` from Ubuntu returned **0% packet loss**, although the ICMP rule had been blocking earlier in the session.


### 11.2 Diagnosis

In Live View, the 1.1.1.1 entries were no longer `LAN In ... block`. The traffic appeared as an outbound **pass** on the WAN interface with source `10.0.3.15` (the firewall's translated address), a TTL of 63 (showing it had been forwarded through the firewall) and the label "let out anything from firewall host itself". That showed the ping was leaving the network and was not being stopped on the LAN side.


### 11.3 Root cause

When the second rule was created and moved, the ICMP rule ended up below the default allow rule (or was no longer above it), so the broad allow rule matched the ping first. Because OPNsense uses the first matching rule from the top, a block rule below an allow rule never takes effect.

### 11.4 Resolution

1. In **Firewall > Rules > LAN**, the rules were reordered so the top of the list was: `LAB2 BLOCK ICMP TO 1.1.1.1`, `LAB2 BLOCK OUTBOUND HTTP`, then the two default allow rules.
2. The changes were applied.
3. Old states for `1.1.1.1` and the client were cleared where needed.

### 11.5 Verification

After the reorder, Live View showed both `LAB2 BLOCK ICMP TO 1.1.1.1` (ICMP) and `LAB2 BLOCK OUTBOUND HTTP` (TCP port 80) entries with the action `block` at the same time (Evidence E5), and the ping to 1.1.1.1 failed again with 100% packet loss.

### 11.6 Lessons learned

- Rule order is part of the rule. Moving or adding one rule can push another below the allow rule, so check the whole list after every change.
- When a block does not work, look at the log entry for the traffic. The action and label show which rule actually made the decision.
- A TTL of 63 on the WAN side, together with the translated source address, is a quick sign that traffic was forwarded through the firewall and not generated by it.
- Always click Apply and confirm the Apply bar has gone before testing.

---

## 12. Analysis Questions

**1. Why must the specific block rules be placed above the broad allow rule?**
OPNsense evaluates interface rules from top to bottom and stops at the first match. The broad "Default allow LAN to any" rule matches all traffic from the LAN. A block rule placed below it would never be reached, so the traffic would be allowed. The block rules must be above it to be evaluated first.

**2. Which five packet attributes are most useful when explaining a firewall decision?**
The five most useful attributes are: the **source address**, the **destination address**, the **protocol** (for example TCP or ICMP), the **destination port** where applicable, and the **interface and direction** (for example LAN, in). Together with the recorded action and rule label, these show exactly which rule matched and why. The timestamp is also useful for linking a log entry to a test.

**3. Why did blocking ICMP not block HTTPS?**
ICMP and HTTPS are different protocols. ICMP is IP protocol 1 and has no ports, while HTTPS uses TCP port 443. The ICMP block rule matched only ICMP packets to 1.1.1.1/32, so HTTPS traffic did not match it and was accepted by the default allow rule.

**4. What difference did you observe between the blocked TCP port 80 traffic and the permitted TCP port 443 traffic?**
The port 80 traffic was blocked: the client sent a SYN, the firewall silently dropped it, no SYN-ACK came back, and the client kept retransmitting the SYN. The port 443 traffic completed the full three-way handshake (SYN, SYN-ACK, ACK) and then carried encrypted TLS data, and the state table showed an ESTABLISHED connection.

**5. What role does outbound NAT play when icdfa-nslab-client-v1 uses a private IPv4 address?**
The client address 10.10.10.77 is private and cannot be routed on the internet. Outbound NAT rewrites the source address to the firewall's WAN address (10.0.3.15, with a different source port) as the packet leaves, and records the translation in the state table so that replies can be mapped back to the original client. Without it, internet servers could not reply to the client.

**6. Why is restoring the original state an important part of a controlled security laboratory?**
Restoring the original state gives the next activity a known-good baseline, so a later fault can be attributed to the new change and not to leftover test rules. It also prevents temporary rules from weakening or breaking the network, keeps results repeatable, and follows good change-control practice. Disabling the rules instead of deleting them preserved the configuration until all evidence was captured.

---

## 13. Conclusion

I recorded a working baseline, created two narrow block rules (ICMP to 1.1.1.1 and outbound TCP port 80), and showed that they blocked only the intended traffic while DNS and HTTPS continued to work. The firewall Live View identified the blocking rules by label, and the Wireshark capture matched those decisions: unanswered ICMP requests and repeated port 80 SYN packets for the blocked traffic, and a completed handshake with TLS for the permitted HTTPS. The state table showed outbound NAT translating the private client address to the firewall WAN address. During the laboratory I found and corrected a rule-order fault that stopped the ICMP rule from taking effect, which reinforced that the first matching rule decides the outcome. Finally, I disabled both test rules and confirmed that ping, HTTP and HTTPS worked again, leaving the laboratory in its original state.

---

## 14. Evidence Checklist

| ID | Evidence | Included |
| --- | --- | --- |
| E1 | Successful baseline ping, DNS, HTTP and HTTPS tests | [ ] |
| E2 | LAN rules page with both LAB2 block rules above the default allow rule | [ ] |
| E3 | Failed ICMP test with successful DNS or HTTPS | [ ] |
| E4 | Failed HTTP test and successful HTTPS test | [ ] |
| E5 | Live View entries for blocked ICMP and TCP port 80 traffic | [ ] |
| E6 | Wireshark evidence: blocked ICMP or HTTP and permitted HTTPS | [ ] |
| E7 | State table for a permitted HTTPS connection (and NAT mode) | [ ] |
| E8 | Final rules page with rules disabled and restored connectivity | [ ] |


---

*International Cybersecurity and Digital Forensics Academy | Student Laboratory Report | WADF105 Practical Laboratory 2*

