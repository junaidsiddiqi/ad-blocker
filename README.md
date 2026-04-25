# 🕳️ Network-Wide DNS Ad Blocker — Pi-hole on Linux Mint

A network-wide DNS filtering system deployed on a repurposed PC running Linux Mint. By configuring Pi-hole as the DNS server for my entire home network, every device — phones, laptops, smart TVs, gaming consoles — has its DNS traffic filtered automatically without installing anything on the devices themselves.

---

## Overview

Instead of installing ad blockers on individual devices, this project takes a network-level approach. Pi-hole acts as a DNS server that intercepts every domain lookup made by any device on the network. If the domain is on a blocklist, Pi-hole returns nothing and the request never reaches the internet. If it's allowed, the request gets forwarded to Cloudflare's DNS resolver (1.1.1.1).

The result is network-wide ad blocking, tracker blocking, and malicious domain filtering that covers every device automatically.

---

## Setup

### 1. Converted Old PC to Linux Mint
- Repurposed an old PC by installing Linux Mint as the operating system
- Chose Linux Mint for its stability and low resource usage as a dedicated DNS server

### 2. Assigned a Static IP Address
- Configured the Pi-hole machine to use a static local IP address
- This is critical — Pi-hole acts as the DNS server for the entire network, so if its IP address changes, every device loses DNS resolution until it's reconfigured
- Without a static IP, DHCP could assign a different address on reboot, breaking the entire setup

### 3. Installed Pi-hole
- Installed Pi-hole on the Linux Mint machine
- Configured **Cloudflare (1.1.1.1)** as the upstream DNS resolver
- This means allowed domains get resolved through Cloudflare instead of the ISP's default DNS, improving both privacy and speed

### 4. Configured the Router
- Set the router's DNS server to the Pi-hole's static IP address
- This is the key step that makes it network-wide — every device that connects to the network automatically has its DNS traffic routed through Pi-hole without any per-device configuration

### 5. Loaded Blocklists
- Added blocklists covering advertising domains, tracking domains, and known malicious domains
- Current blocklist: **87,771 domains**

---

## How It Works

```
Device on Network (phone, laptop, TV, etc.)
        │
        │ DNS query — "what is ads.example.com?"
        ▼
Pi-hole (Linux Mint PC — Static IP: 192.168.4.147)
        │
        ├── Domain on blocklist? → BLOCKED (returns nothing)
        │
        └── Domain allowed? → Forward to Cloudflare (1.1.1.1)
                                        │
                                        ▼
                               Returns IP address to device
```

---

## Results

| Metric | Value |
|--------|-------|
| Total Queries Processed | 145,652 |
| Queries Blocked | 41,079 |
| Block Rate | 28.2% |
| Domains on Blocklist | 87,771 |
| Active Clients | 27 |
| Upstream DNS | Cloudflare (1.1.1.1) |

### Top Blocked Domains
| Domain | Hits | What It Is |
|--------|------|-----------|
| mask.icloud.com | 5,178 | Apple iCloud tracking |
| logs.ads.vungle.com | 4,293 | Mobile ad network |
| beacons.gvt2.com | 1,495 | Google telemetry/tracking |
| beacons.gcp.gvt2.com | 1,479 | Google telemetry/tracking |
| app-measurement.com | 1,400 | Firebase app analytics tracking |

Nearly 28% of all DNS queries on the network were blocked — meaning more than 1 in 4 requests from devices was reaching out to an ad, tracker, or analytics service without the user knowing.

### Top Clients by Traffic
The dashboard shows per-device visibility — every device on the network is tracked by its local IP, showing exactly how many requests each device is making and how many are being blocked.

---

## What I Learned

- **DNS is the foundation of all internet traffic.** Every connection a device makes starts with a DNS query — controlling DNS at the network level means controlling what the entire network can and cannot reach.

- **Network-level filtering is more effective than per-device solutions.** A single Pi-hole instance protects every device simultaneously, including IoT devices and smart TVs that don't support browser extensions or ad blockers.

- **Static IP addressing is essential for network services.** Any device acting as a server needs a predictable, unchanging address. Understanding why this matters gave me a much clearer picture of how network infrastructure works.

- **Devices are constantly phoning home.** Looking at the blocked domains made it clear how much background tracking activity happens without user interaction — Google, Apple, and ad networks are constantly receiving data from devices on the network.

- **Upstream DNS choice matters for privacy.** By routing DNS through Cloudflare instead of the ISP's default resolver, the ISP can no longer log every domain lookup made on the network.

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| Linux Mint | Host operating system |
| Pi-hole | DNS filtering and network-wide ad blocking |
| Cloudflare (1.1.1.1) | Upstream DNS resolver |
| Home Router | DNS pointer to Pi-hole |

---

## Screenshots

> <img width="1568" height="663" alt="image" src="https://github.com/user-attachments/assets/7db3bf66-2162-4a98-aa51-ae592adb3d67" />

> <img width="1269" height="952" alt="image" src="https://github.com/user-attachments/assets/fec0f479-2d8d-4cae-ae25-0eac853e0a00" />

> <img width="725" height="1567" alt="image" src="https://github.com/user-attachments/assets/0e53c286-b2b8-4029-8870-9e2973518934" />

> <img width="992" height="508" alt="image" src="https://github.com/user-attachments/assets/20eed074-27dc-421c-85ac-6748c3f4322f" />

> <img width="987" height="495" alt="image" src="https://github.com/user-attachments/assets/5c5ad169-5436-4770-bdf3-c354074a8e38" />

---

## Disclaimer

This project was deployed on a personal home network for educational purposes and to improve network privacy and performance.
