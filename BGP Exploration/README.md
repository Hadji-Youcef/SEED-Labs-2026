# Autonomous System BGP Routing & Exploration Lab

## 📊 Executive Overview
This laboratory analyzes Wide-Area Network (WAN) routing security and convergence properties using the Border Gateway Protocol (BGP). Utilizing a multi-AS containerized Internet topology driven by the BIRD routing daemon, simulations were executed to study route propagation dynamics, prefix hijacking vulnerabilities, and upstream filtering mitigations.

---

## 🔑 Core Technical Domains

### 1. Autonomous System (AS) Peering & Transit
* **Infrastructure Design:** Investigated BGP attributes including `AS-Path` tracking, `Local Preference`, and `Large Communities` to govern traffic path selection across competing networks.
* **Redundancy Testing:** Tested automated route convergence by forcing connection link failures on primary transit routes, verifying failover to backup peers without route dropping.

### 2. BGP Prefix Hijacking (IP Space Hijacking)
* **Attack Vector:** Configured a rogue Autonomous System (`AS-161`) to announce unauthorized ownership of IP prefixes belonging to a target network (`10.154.0.0/24`).
* **Mechanics:** Analyzed the global propagation of toxic BGP `UPDATE` messages that successfully re-routed global traffic away from legitimate endpoints straight into rogue collectors.
* **Longest Prefix Match Weaponization:** Executed an advanced variant by announcing more specific subnets (`/25`). Routers automatically prioritized the fake paths over the true owner's `/24` route because of longest-mask routing table design, breaking standard path evaluation metrics.

### 3. Upstream Provider-Level Mitigation
* **Import/Export Filters:** Programmed explicit routing evaluation matrices inside the BIRD router engine profiles to instantly drop fraudulent notifications at the border boundary:
    ```bird
    if net = 10.154.0.0/24 then reject;
    ```
* **Defensive Paradigms:** Analyzed structural implementations of programmatic Route Filtering and Resource Public Key Infrastructure (**RPKI**) to systematically validate network announcements before global propagation.

---

## 🛠️ Toolchain Deployment
* **Routing Core:** BIRD Routing Daemon (Configurations, routing tables, and BGP peering sockets).
* **Diagnostics:** SEED internet emulator dashboard, traceroute utilities, dynamic routing tables.
