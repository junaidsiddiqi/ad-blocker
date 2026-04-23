# 🕳️ Network-Wide DNS Ad Blocker — Pi-hole on Linux Mint

A network-wide DNS filtering system deployed on a repurposed PC running Linux Mint, using Pi-hole to intercept and block unwanted and malicious domains across every device on the home network — no per-device configuration required.

---

## Overview

Rather than installing ad blockers on individual devices, this project takes a network-level approach by routing all DNS traffic through a self-hosted Pi-hole instance. Every device connected to the network — phones, laptops, smart TVs, IoT devices — has its DNS queries filtered automatically without any software installed on the devices themselves.

The setup replaces the ISP's default DNS resolver with a local Pi-hole instance, giving full visibility and control over every DNS query made on the network.

---

## Architecture

```
Device on Network (phone, laptop, etc.)
        │
        │ DNS query (e.g. "what is ads.google.com?")
        ▼
Pi-hole (Linux Mint PC — Static IP)
        │
        ├── Domain on blocklist? → BLOCKED (returns null)
        │
        └── Domain allowed? → Forward to Cloudflare (1.1.1.1)
                                        │
                                        ▼
                               Returns IP address
```

---

## Setup

### 1. Converted PC to Linux Mint
- Repurposed an old PC by installing Linux Mint as the operating system
- Chose Linux Mint for its stability and lightweight resource usage as a dedicated DNS server

### 2. Assigned a Static IP Address
- Configured the Pi-hole machine to use a static local IP address
- A static IP is critical here — Pi-hole acts as the DNS server for the entire network, so if its IP address changes, all DNS resolution breaks and every device on the network loses the ability to resolve domain names until it's reconfigured
- Without a static IP, DHCP could assign a different address on reboot, breaking the entire setup

### 3. Installed Pi-hole
- Installed Pi-hole on the Linux Mint machine
- Configured Cloudflare (1.1.1.1) as the upstream DNS resolver, replacing the ISP's default DNS
- This means blocked domains return null, and allowed domains get resolved through Cloudflare's fast and privacy-respecting resolver rather than the ISP's

### 4. Configured the Router
- Set the router's DNS server to the Pi-hole's static IP address
- This is the key step that makes it network-wide — by telling the router to use Pi-hole as the DNS server, every device that connects to the network automatically has its DNS traffic routed through Pi-hole without any per-device changes

### 5. Blocklists
- Loaded [X] domains across [X] blocklists covering ads, trackers, and known malicious domains
- Every DNS query is checked against this list before being forwarded upstream

---

## Results

| Metric | Value |
|--------|-------|
| Total Queries Processed | [X] |
| Queries Blocked | [X] |
| Block Rate | [X]% |
| Domains on Blocklist | [X] |
| Devices Protected | [X] |
| Upstream DNS | Cloudflare (1.1.1.1) |

*Update these values from your Pi-hole dashboard when available*

---

## What Pi-hole Blocks

- Advertising domains (Google Ads, Facebook Ads, etc.)
- Tracking and telemetry domains
- Known malicious domains
- Any custom domains manually added to the blocklist

## What Pi-hole Does NOT Block
- Content within allowed domains (Pi-hole works at the DNS level, not the content level)
- HTTPS traffic content (it only sees the domain name, not what's inside the request)

---

## Key Takeaways

- **DNS is the foundation of all internet traffic.** Every connection a device makes starts with a DNS query — controlling DNS means controlling what your network can and cannot reach.
- **Network-level filtering is more effective than per-device solutions.** A single Pi-hole instance protects every device simultaneously, including IoT devices that don't support browser extensions.
- **Static IP addressing is essential for network services.** Any device acting as a server (DNS, DHCP, etc.) must have a predictable, unchanging address for the network to function reliably.
- **Upstream DNS choice matters.** Using Cloudflare (1.1.1.1) instead of the ISP's default DNS improves privacy since the ISP can no longer log DNS queries.

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| Linux Mint | Host operating system |
| Pi-hole | DNS filtering and ad blocking |
| Cloudflare (1.1.1.1) | Upstream DNS resolver |
| Home Router | DNS server pointer to Pi-hole |

---

## Screenshots

> *Pi-hole Dashboard — Total queries, blocked queries, and per-client activity*

---

## Disclaimer

This project was deployed on a personal home network for educational purposes and to improve network privacy and performance.
