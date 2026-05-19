# SEED Labs: Applied Vulnerability Analysis & Defensive Engineering

This repository contains my personal documentation, notes, and takeaways from working through the hands-on SEED Labs exercises. 

The goal of this repo is to document how these classic network and system vulnerabilities work under the hood, how to analyze them using standard tools, and how to configure proper defenses.

---

## 📂 Structure

* **[`/Firewall_Exploration`](./Firewall_Exploration/)**: LKM-based (`Netfilter`) packet filtering, kernel hooks, stateful packet inspection, and advanced traffic engineering (`iptables` dynamic load balancing).
* **[`/TCP_IP_Attacks`](./TCP_IP_Attacks/)**: Analysis of protocol-layer design flaws, covering SYN flooding, TCP session hijacking, and the integration of reverse shell execution models.
* **[`/Mitnick_Attack`](./Mitnick_Attack/)**: Deep-dive execution of the historical multi-stage 1994 attack vector combining blind connection spoofing, predictable TCP Initial Sequence Numbers (ISNs), and trusted connection manipulation.
* **[`/BGP_Exploration`](./BGP_Exploration/)**: Analysis of Wide-Area Network routing infrastructure, simulating Autonomous System (AS) path manipulation, prefix hijacking, and mitigating via upstream provider filter configurations.

---

## 🛠️ Global Environment & Toolchain
* **Core Infrastructure:** SEED Ubuntu Pre-built Ecosystem / Multi-container Docker Topologies.
* **Analysis & Diagnostics:** Wireshark, GDB, Netcat, Nmap, BIRD Routing Daemon.
* **Packet Manipulation & Scripting:** Python 3, Scapy, C (Kernel Space).

---

## ⚖️ Academic Integrity Compliance
To follow standard academic integrity guidelines, this repository does not contain complete copy-paste exploit code or solutions meant for skipping the labs. It only hosts my own structural write-ups, tool commands, and defensive configuration fixes.
