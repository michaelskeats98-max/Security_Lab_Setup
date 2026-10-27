# Security_Lab_Setup

> **Status: in progress.** I'm building this lab step by step — the progress checklist below tracks what's done and what's left.

A self-directed cybersecurity lab: three virtual machines on an isolated virtual network, used to practise network hardening and vulnerability assessment safely. Built as a portfolio project for industry placement applications.

## What this is & why

A safe, self-contained environment for hands-on network security work — setting up services, hardening them, and measuring the effect with before-and-after scans. Everything runs on an isolated internal network so nothing can reach, or be reached from, the outside world.

## Lab architecture

All three machines share one VirtualBox **Internal Network** called `labnet` (10.10.10.0/24). Only Kali and Ubuntu get a second NAT adapter for updates, disconnected during testing.

| VM | Role | Adapter 1 | Adapter 2 | Lab IP |
|---|---|---|---|---|
| Kali | Attacker / admin box | Internal: `labnet` | NAT (off while testing) | 10.10.10.10 |
| Ubuntu Server | Server I defend: SSH, web, firewall | Internal: `labnet` | NAT | 10.10.10.20 |
| Metasploitable 2 | Deliberately vulnerable target | Internal: `labnet` | *(none)* | 10.10.10.30 |

**Network diagram:** `diagrams/network.png` _(to add)_

## Safety rules

- Metasploitable is vulnerable by design. It is only ever connected to the internal `labnet` network — never NAT or Bridged.
- Only scan or test machines inside this lab. Scanning anything else, including university or work networks, can break the law and institutional rules.

## Tech stack

VirtualBox · Kali Linux · Ubuntu Server 24.04 LTS · Metasploitable 2 · nmap · ufw · nginx · Bash · Git

## Setup summary

1. Add the three VMs to VirtualBox (Kali prebuilt image, Ubuntu from ISO with OpenSSH, Metasploitable from its existing `.vmdk`).
2. Wire every adapter to Internal Network `labnet`.
3. Set static IPs — see [`configs/`](configs/) for the netplan and interfaces files.
4. Verify connectivity, then snapshot each VM as a clean baseline.
5. Run services on Ubuntu (nginx + SSH).
6. Scan, then harden with `ufw` — see before/after scans in [`scans/`](scans/).
7. Assess Metasploitable and document the risks and fixes.

## Progress checklist

### Build
- [x] VirtualBox 7 + Extension Pack installed
- [ ] Kali, Ubuntu Server 24.04, and Metasploitable 2 downloaded
- [ ] All three VMs added to VirtualBox
- [ ] Adapters wired to `labnet` (Metasploitable internal-only)
- [ ] Static IPs set (Kali .10, Ubuntu .20, Metasploitable .30)
- [ ] Connectivity verified (`ping` .20 and .30 from Kali)
- [ ] "Clean baseline" snapshot taken of each VM

### Services, firewall & testing
- [ ] nginx installed on Ubuntu and confirmed with `curl`
- [ ] Baseline scan of Ubuntu saved (`scans/ubuntu-before.txt`)
- [ ] `ufw` rules applied (SSH from Kali only, allow 80, deny the rest)
- [ ] Ubuntu re-scanned (`scans/ubuntu-after.txt`); SSH from Kali still works
- [ ] Port 80 rule removed, re-scanned, port confirmed filtered
- [ ] Metasploitable scanned (`scans/metasploitable.txt`)
- [ ] Three risky services picked, each with a one-line fix

### Documentation
- [ ] GitHub repo created with folder structure
- [ ] Network diagram added to `diagrams/`
- [ ] Config files committed to `configs/`
- [ ] Scan outputs committed to `scans/`
- [ ] Screenshots added to `screenshots/`
- [ ] Troubleshooting log kept up to date
- [ ] Findings section below completed
- [ ] CV line finalised

## Findings

_To be completed as the lab progresses._

- **Firewall hardening (before vs after):** _summary of what the `ufw` rules closed, with the before/after scans in `scans/`._
- **Metasploitable assessment:** _three high-risk services (e.g. unencrypted Telnet/FTP, outdated Apache), each with why it's risky and how I'd fix it — patch, disable, or firewall._

