# Firewall Exploration Lab

## 📊 Executive Overview
This laboratory covers the mechanics of packet filtration and stateful access control across two distinct paradigms: low-level Linux Kernel Modules (LKMs) interacting with the kernel's network stack via Netfilter hooks, and programmatic packet engineering via standard user-space utilities (`iptables`). Advanced traffic controls—specifically rate-limiting and dynamic layer-4 load balancing—were implemented and measured.

---

## 🔑 Core Technical Domains

### 1. Netfilter Hooks & Linux Kernel Modules (LKMs)
* **Architecture:** Developed structural C kernel code to register memory callbacks within the Netfilter system at key hook locations (`NF_INET_PRE_ROUTING`, `NF_INET_LOCAL_IN`).
* **Granular Dropping:** Implemented custom kernel rules to block UDP traffic hitting specific destination sockets (e.g., `8.8.8.8:53`).
* **Operational Risk:** Handled kernel memory spaces directly, ensuring safety parameters to prevent Null Pointer Dereferences or Kernel Panics during heavy traffic volume.

### 2. User-Space Packet Filtering with `iptables`
* **Stateless Configurations:** Designed rulesets targeting specific infrastructure layouts to safely isolate routers, corporate networks, and public-facing dmz nodes.
* **Stateful Connection Tracking:** Deployed rules utilizing the `conntrack` framework (`ctstate`) to recognize existing `ESTABLISHED` and `RELATED` parameters. This allows safe, outbound communication models while dropping unauthorized external inputs cleanly.

### 3. Traffic Engineering & Load Balancing
* **Dynamic Round-Robin (`nth` Mode):** Configured automated traffic distribution over internal backend instances using `statistic --mode nth`. Verified perfect 1:1 packet balancing via targeted metrics.
* **Probabilistic Routing (`random` Mode):** Developed mathematical rule chains to distribute incoming load using decreasing sequential weight allocations:
    $$\text{Rule 1 } P(A) = 33\%, \quad \text{Rule 2 } P(B|A') = 50\% \implies 33\%, \quad \text{Rule 3 Default} \implies 34\%$$
    Verified long-tail convergence towards equilibrium under volume simulation.

---

## 🛠️ Toolchain Deployment
* **Languages:** C (Kernel Space APIs), Bash.
* **Utilities:** `iptables`, `iptables-save`, `conntrack`, `curl`.
* **Environment:** Multi-interface Ubuntu routing containers.
