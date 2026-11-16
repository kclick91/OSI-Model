# The OSI Model: A Beginner's Guide

## What Is the OSI Model?

The **Open Systems Interconnection (OSI) model** is a conceptual framework that describes how data moves between devices across a network. It was developed by the International Organization for Standardization (ISO) and is formalized in **ISO/IEC 7498-1**. It splits networking into **seven layers**, each with one job, so engineers can design, troubleshoot, and secure systems piece by piece.

> **A note on "modern":** The seven-layer OSI model itself has not changed since its standardization in the 1980s and 1990s. What has changed is how we apply it: cloud, containers, 5G, IoT, zero trust, and AI all map onto the same seven layers. Real-world networks run on the simpler **TCP/IP model** (4 to 5 layers), but OSI remains the shared vocabulary used in security, certification exams, and troubleshooting.

**Memory trick (Layer 7 down to 1):** *All People Seem To Need Data Processing*
**(Layer 1 up to 7):** *Please Do Not Throw Sausage Pizza Away*

---

## Quick Reference

| # | Layer | Job | Data Unit | Example Protocols / Tech |
|---|-------|-----|-----------|--------------------------|
| 7 | Application | Interface for apps and users | Data | HTTP/HTTPS, DNS, SMTP, FTP |
| 6 | Presentation | Format, encrypt, compress | Data | TLS, JPEG, JSON, UTF-8 |
| 5 | Session | Start, manage, end sessions | Data | RPC, NetBIOS, session tokens |
| 4 | Transport | End-to-end delivery, reliability | Segment / Datagram | TCP, UDP, QUIC |
| 3 | Network | Addressing and routing | Packet | IP (IPv4/IPv6), ICMP, IPsec |
| 2 | Data Link | Node-to-node delivery on a local link | Frame | Ethernet, Wi-Fi (802.11), ARP |
| 1 | Physical | Raw bits over a medium | Bits | Fiber, copper, radio, hubs |

---

## Layer-by-Layer Explanation

> **Research snapshot (as of November 16, 2026):** Each layer below ends with three examples of major current research. Some are standards efforts, some are academic papers, and some come from industry reports, so check the linked sources at the end for details. Several items (Wi-Fi 8, 6G, ASPA) are still being developed or adopted.

### Layer 7: Application
**What it does:** Provides network services directly to software the user interacts with. It is not the app itself (like Chrome) but the protocols the app uses.

**In practice:** Loading a website uses **HTTPS**, sending email uses **SMTP**, and typing `google.com` triggers a **DNS** lookup.

**Cybersecurity relevance:** The most commonly attacked layer. Threats include SQL injection, cross-site scripting (XSS), API abuse, phishing, and DDoS attacks targeting web servers. Defenses include web application firewalls (WAFs), input validation, and API gateways.

**Current research (3 examples):**
1. **Tool poisoning and prompt injection through the Model Context Protocol (MCP).** Researchers are studying how malicious tool descriptions and tool results can hijack AI agents, and a 2026 Cloud Security Alliance note examines design-level risks in MCP itself, including prompt injection that escalates to remote code execution.
2. **Credential theft from AI coding agents.** 2026 disclosures showed that crafted GitHub comments, pull request titles, and issue text could steer coding agents into running commands or leaking API keys, and Johns Hopkins researchers reported hijacking agents from three major vendors.
3. **Persistent memory poisoning in agents.** Security databases now catalog attacks (such as Logic-layer Prompt Control Injection) where instructions are planted in an agent's long-term memory or retrieval pipeline so they survive across sessions.

---

### Layer 6: Presentation
**What it does:** Translates data between the application and the network: encoding, compression, and encryption.

**In practice:** **TLS** encrypts your browser traffic, images are compressed to JPEG, and data is serialized as JSON or XML.

**Cybersecurity relevance:** Encryption lives here conceptually. Risks include weak ciphers, expired or forged certificates, downgrade attacks, and malformed-data exploits. Defenses include modern TLS 1.3, certificate management, and strict parsing.

**Current research (3 examples):**
1. **Measuring the post-quantum shift in TLS.** A 2026 internet measurement study found that roughly half of the domains it analyzed already support hybrid key exchange (X25519 plus ML-KEM-768), while the rest remain fully classical.
2. **Formal analysis of hybrid vs. standalone ML-KEM.** Papers such as "FATT Chance" (IACR ePrint 2026/1147) and IETF drafts examine whether using ML-KEM alone in TLS 1.3 is as robust as combining it with classical key exchange.
3. **Operationalizing post-quantum migration.** A Nanyang Technological University team automated the profiling of 8,443 real Nginx configurations and piloted hybrid deployment at a financial institution, while IETF drafts explore lighter ML-KEM-512 hybrids for constrained devices.

---

### Layer 5: Session
**What it does:** Opens, maintains, and closes conversations between two systems, including checkpoints and reconnection.

**In practice:** Staying logged in to a web app, a video call that reconnects after a dropped signal, or remote procedure calls (RPC).

**Cybersecurity relevance:** Attackers target sessions through **session hijacking**, token theft, and replay attacks. Defenses include short-lived tokens, secure cookies, multi-factor authentication, and session timeouts.

**Current research (3 examples):**
1. **Device Bound Session Credentials (DBSC).** Developed by Google with Microsoft's collaboration, DBSC ties a session to a hardware-held key so a stolen cookie is useless elsewhere; it reached general availability in Chrome 146 for Windows in April 2026.
2. **Attacks that bypass device binding.** In August 2026, SpecterOps released tools that abuse Chrome's DevTools Protocol to hijack a live session from inside the victim's own browser, showing that binding stops cookie replay but not on-device abuse.
3. **Extending binding to federated login and more devices.** Google is working on cross-origin bindings so single sign-on sessions stay tied to the same device key, and is exploring software-based keys for devices without secure hardware.

---

### Layer 4: Transport
**What it does:** Delivers data between processes on two hosts using **ports**. It handles segmentation, flow control, and (optionally) reliability.

**In practice:** **TCP** is reliable and ordered (web, email, file transfer). **UDP** is fast and connectionless (video streaming, gaming, DNS). **QUIC** (used by HTTP/3) runs over UDP with built-in encryption.

**Cybersecurity relevance:** Port scanning, SYN flood DoS attacks, and UDP amplification attacks occur here. Defenses include stateful firewalls, rate limiting, and closing unused ports.

**Current research (3 examples):**
1. **Multipath QUIC.** The IETF extension that lets one QUIC connection use several paths at once (for example Wi-Fi and cellular) was reported in November 2026 as published as RFC 10069, and research continues on smarter packet schedulers.
2. **Learning-based congestion control for QUIC.** Reinforcement-learning approaches, such as MVFST-RL and the plug-and-play AERO algorithm, aim to replace hand-tuned heuristics for adjusting sending rates.
3. **Ultra Ethernet Transport (UET) for AI clusters.** The Ultra Ethernet Consortium's transport uses out-of-order delivery and multipath spraying for massive AI and HPC networks, and version 1.1 of the specification (October 2026) added scale-up networking, in-network collectives, and programmable congestion control.

---

### Layer 3: Network
**What it does:** Assigns logical addresses (IP addresses) and routes packets across different networks.

**In practice:** Your home router forwarding traffic to the internet, **IPv4/IPv6** addressing, `ping` and `traceroute` (ICMP), and **VPNs** using IPsec.

**Cybersecurity relevance:** IP spoofing, route hijacking (BGP attacks), and ICMP-based attacks. Defenses include network firewalls, access control lists (ACLs), network segmentation, and IPsec VPNs.

**Current research (3 examples):**
1. **ASPA path verification.** Autonomous System Provider Authorization, defined in IETF drafts, lets networks publish their authorized upstream providers so route leaks can be detected, which origin validation alone cannot do; adoption was still only a few percent or less in 2026 measurements.
2. **Why RPKI-invalid routes persist.** A Virginia Tech and CableLabs study examined why invalid routes have not declined even as signed route coverage has grown, partly because of misconfigurations and noisy detection.
3. **Testing and observing ASPA in real routers.** Researchers compared how the BIRD and OpenBGPD routing daemons validate paths, and RIPE Labs published an open-source tool (RAVEN) that labels live routes by their origin and ASPA validation state.

---

### Layer 2: Data Link
**What it does:** Moves frames between devices on the same local network using **MAC addresses**, and detects errors.

**In practice:** Ethernet cables, Wi-Fi, switches, and **ARP** (matching IP addresses to MAC addresses). VLANs also operate here.

**Cybersecurity relevance:** ARP spoofing, MAC flooding, rogue access points, and Wi-Fi attacks such as evil twins. Defenses include WPA3, port security, 802.1X authentication, and VLAN segmentation.

**Current research (3 examples):**
1. **AirSnitch (NDSS 2026).** Researchers from UC Riverside and KU Leuven showed that Wi-Fi client isolation can be bypassed on all 11 router and firmware images they tested, enabling man-in-the-middle attacks even on WPA2/WPA3 networks, because client isolation is not defined in the IEEE 802.11 standard.
2. **Wi-Fi 8 (IEEE 802.11bn, Ultra High Reliability).** Rather than chasing peak speed, this effort targets consistent performance using multi-link operation and multi-access-point coordination; Draft 2.0 went to ballot in mid-2026 and final approval is projected for 2028.
3. **Lossless link layers for AI fabrics.** Ultra Ethernet's link layer adds features such as link-level retry and credit-based flow control, and its July 2026 specification update fixed race conditions between the two.

---

### Layer 1: Physical
**What it does:** Transmits raw bits (electrical, optical, or radio signals) over a physical medium.

**In practice:** Ethernet cables, fiber optics, radio waves, network interface cards, and cell towers.

**Cybersecurity relevance:** Wiretapping, cable cutting, signal jamming, hardware implants, and unauthorized physical access. Defenses include locked server rooms, shielded cabling, tamper-evident hardware, and surveillance.

**Current research (3 examples):**
1. **Integrated sensing and communication (ISAC).** 3GPP Release 20 studies are exploring cellular radios that also act as sensors, such as base stations detecting drones, as groundwork for 6G.
2. **Reconfigurable intelligent surfaces (RIS).** Programmable surfaces that reshape radio propagation are being studied for coverage and for physical-layer security, including the risk of malicious RIS.
3. **Satellite access without GNSS.** A Release 20 study is assessing how non-terrestrial networks can handle initial access and connected-mode operation without relying on GPS-style positioning.

---

## How Data Flows: Encapsulation

When you send data, it moves **down** the layers on your device, with each layer adding its own header (**encapsulation**). The receiving device moves it **up** the layers, removing headers (**decapsulation**).

```
Sender                              Receiver
[7] Application   ---------------   [7] Application
[6] Presentation  ---------------   [6] Presentation
[5] Session       ---------------   [5] Session
[4] Transport     ---------------   [4] Transport
[3] Network       ---------------   [3] Network
[2] Data Link     ---------------   [2] Data Link
[1] Physical      ====== bits ====  [1] Physical
```

---

## The OSI Model and Cybersecurity

The OSI model gives defenders a **structured way to think about "defense in depth"**: security controls are placed at multiple layers so one failure does not compromise everything.

| Layer | Example Attack | Example Defense |
|-------|----------------|-----------------|
| 7 | SQL injection, phishing | WAF, input validation |
| 6 | Weak or stolen TLS certificates | TLS 1.3, certificate pinning |
| 5 | Session hijacking | Secure tokens, MFA |
| 4 | SYN flood, port scan | Stateful firewall, rate limiting |
| 3 | IP spoofing, BGP hijack | ACLs, IPsec, segmentation |
| 2 | ARP spoofing, rogue Wi-Fi | WPA3, 802.1X, port security |
| 1 | Cable tapping, jamming | Physical access control |

**Why it helps beginners:** When something breaks or is attacked, you can ask "*which layer is this happening at?*" and narrow down the cause quickly. Modern concepts like **zero trust**, **microsegmentation**, and **SASE** are essentially strategies for enforcing security across several OSI layers at once.

---

## How Artificial Intelligence Affects the OSI Model

AI does not add new layers, but it changes how each layer is **attacked, defended, and managed**.

### AI as a Defensive Tool
- **Layers 3 and 4:** Machine learning detects anomalous traffic patterns, DDoS floods, and port scans in real time.
- **Layer 7:** AI-powered WAFs and email filters spot phishing, malicious payloads, and bot traffic.
- **Layers 1 and 2:** AI analyzes signal behavior to detect rogue access points and jamming.
- **Across all layers:** AI-driven security platforms (SIEM, XDR, NDR) correlate events from many layers to find attacks faster and reduce analyst workload.

### AI as an Offensive Tool
- Attackers use generative AI to craft convincing **phishing** and deepfake voice/video scams (Layer 7).
- AI automates **reconnaissance**, vulnerability scanning, and exploit development (Layers 3 to 7).
- **Adversarial machine learning** can trick AI-based detection systems into misclassifying malicious traffic.

### AI Systems as New Attack Surfaces
- AI services run through the same stack: APIs over HTTPS (Layer 7), TLS (Layer 6), and TCP/QUIC (Layer 4). They inherit all of its vulnerabilities.
- New Layer 7 threats include **prompt injection**, model theft, data poisoning, and insecure AI agent or tool connections.
- Large AI training clusters depend on very high-speed networks (such as InfiniBand and RDMA), which push the limits of Layers 1 to 4.

### AI in Network Operations
- **Intent-based and self-healing networks** use AI to configure routing (Layer 3), balance loads (Layer 4), and optimize wireless channels (Layers 1 and 2).
- **5G/6G and IoT** networks use AI to manage huge numbers of devices and allocate bandwidth dynamically.

---

## Summary (15 Sentences)

1. The OSI model is a seven-layer framework that explains how data travels between devices on a network.
2. Its layers, from top to bottom, are Application, Presentation, Session, Transport, Network, Data Link, and Physical.
3. The top three layers handle user-facing services, data formatting and encryption, and the management of conversations between systems.
4. The Transport and Network layers deliver data between applications and route packets across networks using ports and IP addresses.
5. The Data Link and Physical layers move frames across a local link and send raw bits over cables, fiber, or radio.
6. Data is encapsulated as it moves down the layers on the sender's device and decapsulated as it moves up on the receiver's device.
7. Real networks mostly run on the simpler TCP/IP model, but OSI remains the standard vocabulary for learning and troubleshooting.
8. Each layer faces distinct cyber threats, so defense in depth places controls such as WAFs, firewalls, encryption, and physical security at multiple layers.
9. Thinking in layers helps defenders quickly locate where an attack or failure is occurring.
10. AI strengthens defense through anomaly detection and automated threat correlation, while also giving attackers tools for better phishing, faster reconnaissance, and evasion.
11. AI also creates new attack surfaces, such as prompt injection and data poisoning, and reshapes how networks are monitored, secured, and managed.
12. Current research at the upper layers focuses on securing AI agents (MCP and prompt injection), post-quantum TLS, and device-bound sessions.
13. In the middle layers, researchers are advancing Multipath QUIC, learning-based congestion control, Ultra Ethernet Transport for AI clusters, and ASPA route-leak protection.
14. At the lower layers, work includes Wi-Fi 8, the AirSnitch findings on client isolation, and 6G topics such as sensing, intelligent surfaces, and satellite access.
15. Overall, the seven layers themselves are likely to remain stable, while ongoing research and plausible future improvements, such as cross-layer identity binding, agent-aware protocols, and cryptographic agility, will keep changing what runs at each layer and how it is secured.

---

## Five Plausible Innovations and Improvements to the OSI Model

> **Note:** These are forward-looking ideas, not adopted standards. They are informed by the research trends above, and the OSI model's seven layers could stay the same while these changes happen *inside* and *across* them.

| # | Innovation | Layers Affected | Main Problem It Addresses |
|---|-----------|-----------------|---------------------------|
| 1 | Cross-layer identity binding | 1, 2, 3 | Spoofing and isolation bypass |
| 2 | An agent-aware sublayer for AI tools | 5, 6, 7 | Prompt injection and over-privileged AI agents |
| 3 | Built-in cryptographic agility | 2, 3, 4, 6 | Slow, painful algorithm migrations (such as post-quantum) |
| 4 | Standardized cross-layer telemetry and signaling | 2, 3, 4 | Blind spots between layers and noisy security alerts |
| 5 | A programmable, sensing-aware physical layer | 1, 2 | Fixed radios, privacy risks, and unreliable wireless links |

### 1. Cross-Layer Identity Binding
**Idea:** Cryptographically tie a device's identity at every layer together (its physical port or radio link, its MAC address, its IP address, and its session keys) so none can be impersonated alone.

**Why it is plausible:** Attacks like AirSnitch work because a client's identity at Layer 1/2 is not cryptographically bound to its identity at higher layers. Standardizing client isolation and binding identities (for example, by extending MACsec-style link protection and 802.1X) would close that gap.

**Challenges:** Backward compatibility with billions of existing devices, and roaming and mesh networks where identities legitimately move.

### 2. An Agent-Aware Sublayer for AI Tools (Layers 5 to 7)
**Idea:** A standardized sublayer for how AI agents connect to tools and data, with built-in authentication, authorization, provenance tagging (who wrote this content?), and least-privilege permissions per tool call.

**Why it is plausible:** Protocols like MCP are already acting as an "agent session layer," and current research shows tool descriptions and tool results can carry malicious instructions. Treating agent-to-tool traffic as its own well-defined layer would let security controls (such as separating *data* from *instructions*) be built into the protocol instead of bolted on.

**Challenges:** Agreeing on a standard across vendors, and the fact that prompt injection stems partly from how language models process text, so protocol fixes alone may not be enough.

### 3. Built-In Cryptographic Agility
**Idea:** Make it routine to swap cryptographic algorithms without redesigning protocols, using negotiated, inventory-tracked, and automatically updatable crypto at the Presentation layer and in security protocols at Layers 2, 3, and 4 (MACsec, IPsec, QUIC).

**Why it is plausible:** The post-quantum transition shows how hard migration is: hybrid ML-KEM key exchange is deployed in TLS, but signatures, certificates, VPNs, and legacy systems lag behind. Automated cryptographic inventories and hybrid modes are early steps toward making agility a standard feature.

**Challenges:** Larger keys and handshakes on constrained or lossy links, downgrade attacks, and long-lived embedded devices that cannot be updated.

### 4. Standardized Cross-Layer Telemetry and Signaling
**Idea:** A common, secure way for layers to share signals, such as congestion, path quality, validation status, and attack indicators, with each other and with AI-based management systems.

**Why it is plausible:** Today a route can be security-valid but performing badly, and defenders must stitch together logs from many layers. Efforts like congestion signaling in Ultra Ethernet and tools that annotate routes with RPKI and ASPA status show the building blocks. AI systems could then use these signals for self-healing networks and faster, more accurate anomaly detection.

**Challenges:** Privacy and security of the telemetry itself (it becomes an attack target), vendor interoperability, and avoiding feedback loops where automated reactions make things worse.

### 5. A Programmable, Sensing-Aware Physical Layer
**Idea:** Treat Layer 1 as software-defined and multi-purpose: radios that adapt their waveforms, steer signals using reconfigurable intelligent surfaces, and double as sensors (integrated sensing and communication), with security and privacy rules built in.

**Why it is plausible:** 6G studies in 3GPP Release 20 already explore sensing and intelligent surfaces, and Wi-Fi 8 focuses on reliability through multi-link and multi-AP coordination. A physical layer that can reconfigure itself could improve reliability and coverage, and could include physical-layer security features.

**Challenges:** Radio sensing can reveal people's movements, so privacy protections are essential, and malicious or compromised reconfigurable surfaces are a new risk.

---

## Research Sources (accessed November 2026)

**Layer 7:** [CSA: MCP design-level RCE](https://labs.cloudsecurityalliance.org/research/csa-research-note-mcp-design-rce-protocol-attack-surface-202/) | [AI agent security incidents](https://futureagi.com/blog/ai-agent-security-risks/) | [Agent credential leaks](https://www.cequence.ai/?p=25390) | [Prompt-injection database](https://promptfoo.dev/lm-security-db/tag/application-layer/?tags=api)

**Layer 6:** [PQ readiness of the Internet (arXiv)](https://arxiv.org/pdf/2606.16473) | [FATT Chance (ePrint 2026/1147)](https://eprint.iacr.org/2026/1147) | [Operationalising PQ TLS (ePrint 2026/959)](https://eprint.iacr.org/2026/959) | [ECDHE-MLKEM512 draft](https://datatracker.ietf.org/doc/draft-rosomakho-tls-ecdhe-mlkem512/)

**Layer 5:** [DBSC in Chrome 146](https://www.helpnetsecurity.com/2026/04/10/google-chrome-device-bound-session-credentials/) | [CSA: CDP session hijacking](https://labs.cloudsecurityalliance.org/research/csa-research-note-chrome-devtools-session-hijacking-20260817/)

**Layer 4:** [RFC 10069 announcement (Japanese)](https://asnokaze.hatenablog.com/entry/2026/11/09/000339) | [MVFST-RL](https://arxiv.org/pdf/1910.04054) | [AERO](https://www.citedrive.com/en/discovery/towards-a-pluggable-and-implementation-agnostic-learning-based-congestion-control-for-quic/) | [Ultra Ethernet 1.1](https://www.windowsmode.com/?p=47976)

**Layer 3:** [IETF ASPA verification draft](https://datatracker.ietf.org/doc/draft-ietf-sidrops-aspa-verification/27/) | [RPKI-invalid routes](https://www.noction.com/?p=27018678) | [NLnet Labs ASPA study](https://nlnetlabs.nl/downloads/publications/RP2_Report_ASPA.pdf) | [RIPE Labs: ASPA is live](https://labs.ripe.net/author/ritesh-mukherjee/aspa-is-live-can-you-see-it-working/)

**Layer 2:** [AirSnitch](https://cyberinsider.com/new-airsnitch-attack-bypasses-client-isolation-in-wi-fi-networks/) | [Wi-Fi 8 outlook (ABI Research)](https://www.abiresearch.com/market-research/insight/7788568-wi-fi-8-presents-both-lucrative-opportunit) | [UE Spec 1.0.3 release notes](https://ultraethernet.org/wp-content/uploads/sites/20/2026/07/UE-Specification-1.0.3-release-notes-.pdf)

**Layer 1:** [ISAC in 3GPP (arXiv)](https://arxiv.org/pdf/2608.15283) | [RIS for physical-layer security](https://arxiv.org/pdf/2311.08112) | [3GPP Release 20 overview](https://www.rcrwireless.com/?p=424655)

---

## Further Learning

- ISO/IEC 7498-1 (the OSI Basic Reference Model)
- Wireshark: free tool for viewing real packets and identifying their layers
- Cisco Packet Tracer: free network simulator
- CompTIA Network+ and Security+ study materials (both teach the OSI model heavily)
