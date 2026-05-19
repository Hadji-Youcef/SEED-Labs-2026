# The Historical Mitnick Attack Lab

## 📊 Executive Overview
A strict reconstruction of Kevin Mitnick's 1994 multi-stage attack vector against Tsutomu Shimomura’s infrastructure. The project evaluates the catastrophic compounding effect of multiple unmitigated design assumptions: predictable Initial Sequence Numbers (ISNs), source IP spoofing, and address-based authorization schemas.

---

## 🔑 Core Technical Domains

### 1. SYN Flooding of Trusted Systems
* **Isolation Strategy:** Prior to spoofing the primary target, the trusted secondary server (`Trusted Server`) was disabled using a structured `SYN` flood. 
* **Objective:** Saturated the trusted host's TCB queues, preventing it from processing or responding with a `RST` (Reset) packet when it subsequently received fraudulent state alerts from the target machine.

### 2. Blind TCP Sequence Number Prediction
* **State Reconstruction:** Deployed packet analysis to model the target's Linear Congruential Generator (LCG) or increment frequency for Initial Sequence Numbers ($ISN$).
* **Blind Forging:** Spoofed a `SYN` request pretending to originate from the silenced `Trusted Server`. Without seeing the target’s outbound `SYN-ACK` reply, a calculated `ACK` packet was injected blindly with a guessed sequence parameter:
    $$ACK_{val} = ISN_{target} + 1$$
    Successfully transitioned the socket into an `ESTABLISHED` state from a completely spoofed perspective.

### 3. Trust Exploitation & Backdoor Instantiation
* **Protocol Abuse:** Utilized the authenticated connection state to pass instructions into the Remote Shell (`rsh`) daemon.
* **Persistent Compromise:** Remotely altered target authorization profiles by appending a universal wildcard entry (`+ +`) into the target's local system security config files (`.rhosts`). This neutralized all address constraints and permanently dropped authentication checks for upcoming raw access.

---

## 🔑 Primary Structural Countermeasures
| Vulnerability Element | Defensive Clean Fix | Technical Baseline |
| :--- | :--- | :--- |
| **Predictable ISNs** | Cryptographic Sequence Randomization | Modern Linux Stack (RFC 6528 Implementation) |
| **Address Authentication** | Complete deprecation of `.rhosts` schemas | Transition to cryptographic SSH keys |
| **IP Spoofing** | Gateway Edge Packet Auditing | Ingress Filtering policies (BCP 38) |

---

## 🛠️ Toolchain Deployment
* **Stack:** Python 3, Scapy packet generator, Docker networking framework.
* **Logging:** Wireshark pcap traces proving sequence alignment and connection stability.
