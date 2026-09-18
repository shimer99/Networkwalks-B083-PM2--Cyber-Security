# Networkwalks-B083-PM2--Cyber-Security

# 🛡️ Cybersecurity Assessment: External Footprinting & Subnet Scanning

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Platform: Kali Linux | Windows](https://img.shields.io/badge/Platform-Kali%20Linux%20%7C%20Windows-lightgrey.svg)
![Audit Status: Completed](https://img.shields.io/badge/Status-Completed-success.svg)

A cybersecurity reconnaissance and network auditing project documenting passive/active footprinting against an external web property (`networkwalks.com`) and Layer-2 host discovery and topology mapping across a local subnet (`192.168.0.0/24`).

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Engagement Scope](#-engagement-scope)
- [Methodology & Tools](#-methodology--tools)
- [Phase 1: External Footprinting (`networkwalks.com`)](#-phase-1-external-footprinting-networkwalkscom)
- [Phase 2: Local Subnet Discovery (`192.168.0.0/24`)](#-phase-2-local-subnet-discovery-1921680024)
- [Key Findings & Summary Table](#-key-findings--summary-table)
- [Repository Structure](#-repository-structure)
- [How to Reproduce](#-how-to-reproduce)
- [Disclaimer](#-disclaimer)

---

## 🔍 Overview

This assessment establishes an intelligence baseline for security testing and vulnerability evaluation. It was conducted in two primary stages:

1. **External Footprinting & OSINT:** Gathering public domain governance records, DNS infrastructure mappings, web runtime configurations, and firewall protections for `networkwalks.com`.
2. **Internal Network Scanning & Discovery:** Executing ping sweeps, ARP resolution, and radial topology mapping via **Nmap / Zenmap** on a local Class C subnet (`192.168.0.0/24`).

---

## 🎯 Engagement Scope

| Parameter | Details |
| :--- | :--- |
| **External Target** | `networkwalks.com` |
| **Internal Subnet** | `192.168.0.0/24` (RFC 1918 Private LAN) |
| **Primary Platforms** | Kali Linux (Rolling) & Windows 10/11 |
| **Scanning Engine** | Zenmap / Nmap v7.991 |
| **Assessment Standard** | OSSTMM & OWASP Information Gathering Guidelines |

---

## 🛠️ Methodology & Tools

- **Passive Intelligence / WHOIS:** Domain registrar validation, registration dates, privacy proxy identification.
- **DNS Enumeration (`nslookup`, `dnsrecon`):** Record mapping (A, NS, MX, TXT, SPF, SRV) and zone behavior analysis.
- **Web Stack Profiling (`curl -I`, `whatweb`):** Protocol inspection, response headers, caching layers, CMS identification.
- **WAF Inspection (`wafw00f`):** Behavioral rule detection and Web Application Firewall profiling.
- **Subnet Ping Sweep (`zenmap` / `nmap -sn`):** Live endpoint discovery, latency measurement, MAC/OUI hardware mapping, and radial topology diagram generation.

---

## 🌐 Phase 1: External Footprinting (`networkwalks.com`)

### 1. WHOIS Intelligence
- **Registrar:** GoDaddy.com, LLC (IANA ID: `146`)
- **Creation Date:** 2019-11-06
- **Registry Expiry:** 2027-11-06
- **Registrant Organization:** Domains By Proxy, LLC (Tempe, AZ, US)
- **Status Locks:** `clientTransferProhibited`, `clientUpdateProhibited`, `clientRenewProhibited`, `clientDeleteProhibited`
- **DNSSEC:** Unsigned

### 2. DNS Zone Architecture
- **Apex A Record:** `192.232.216.135` (HostGator / Unified Layer)
- **Nameservers:** 
  - `ns6135.hostgator.com` (`50.87.144.87`) — BIND Version `9.16.23-RH`
  - `ns6136.hostgator.com` (`192.232.216.131`) — BIND Version `9.16.23-RH`
- **Mail Exchanger (MX):** `mail.networkwalks.com` -> `192.232.216.135`
- **SPF Record:** `v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all`
- **SRV Records:** 8 cPanel autodiscover endpoints mapped over port 443

### 3. Application Stack & WAF Protection
- **Web Server:** Apache (HTTP/2 enabled)
- **Caching Layer:** Nginx reverse proxy (`x-nginx-cache: WordPress`, `x-endurance-cache-level: 0`)
- **CMS:** WordPress 7.1.1, Bootstrap 7.1.1, jQuery 3.7.1, WP Download Manager 3.3.58
- **Session Security:** Cookie `__wpdm_client` flagged `Secure` and `HttpOnly`
- **WAF Detection:** **ModSecurity (SpiderLabs)** active and inspecting ingress traffic

---

## 🖥️ Phase 2: Local Subnet Discovery (`192.168.0.0/24`)

A ping sweep (`nmap -sn 192.168.0.0/24`) completed in **4.79 seconds**, detecting **6 live hosts** within the local segment:

| IP Address | Latency | Hardware MAC | Vendor / Role |
| :--- | :--- | :--- | :--- |
| `192.168.0.1` | 0.022s | `E4:FA:C4:1E:EF:B3` | TP-Link Systems (Default LAN Gateway) |
| `192.168.0.101` | 0.000s | *Localhost Interface* | Auditing Workstation (Scanning Host) |
| `192.168.0.103` | 0.062s | `62:CD:CC:E8:C3:27` | Unknown / Randomized MAC (Mobile Host) |
| `192.168.0.125` | 0.075s | `A6:E8:4C:5A:E7:85` | Unknown / Mobile or Workstation |
| `192.168.0.128` | 0.180s | `4A:9D:6F:E8:7B:78` | Unknown / Wireless Endpoint |
| `192.168.0.197` | 0.039s | `16:85:F5:99:DC:51` | Unknown / Active Endpoint |

### 🕸️ Topology Visualization
The radial topology exported from Zenmap confirms `localhost` (`192.168.0.101`) situated at the center hub, maintaining active one-hop links to the default gateway and all five peer nodes.

---

## 📊 Key Findings & Summary Table

| Category | Finding | Security Implication |
| :--- | :--- | :--- |
| **DNSSEC** | Unsigned | Possible DNS spoofing / cache poisoning vulnerability |
| **BIND Version** | `9.16.23-RH` Disclosed | Banner leakage assists targeted exploit profiling |
| **Email SPF** | SoftFail (`~all`) | Permits spoofed inbound mail under non-strict configurations |
| **Perimeter WAF** | ModSecurity Active | Shields common Layer-7 injection and automated scans |
| **Local Network** | Flat `/24` Subnet | Missing VLAN segmentation between management, clients, and IoT |

---

# Author 
Mohamed Rafeek Mohamed Shimer
Linked in: www.linkedin.com/in/rafeek-shimer

