# VSFTPd 2.3.4 Backdoor Exploitation Lab

## Overview
Penetration test of a vulnerable FTP service (VSFTPd 2.3.4) running on Metasploitable2 in an isolated lab environment. This project demonstrates a full attack chain from reconnaissance to post-exploitation.

## Lab Environment
- **Attacker:** Kali Linux (192.168.100.68)
- **Target:** Metasploitable2 (192.168.100.67)
- **Network:** Isolated virtual network (VMware Fusion + UTM)

## Tools Used
- `nmap` — service discovery and version detection
- `Metasploit Framework` — exploit research
- `netcat (nc)` — manual backdoor exploitation
- `John the Ripper` — password hash cracking

## Attack Chain

### 1. Reconnaissance
Ran nmap service scan to identify open ports and software versions on the target.
Discovered VSFTPd 2.3.4 running on port 21 — a version known to contain a backdoor.

### 2. Vulnerability Identification
CVE-2011-2523 — VSFTPd 2.3.4 contains a backdoor that is triggered by sending a username containing the string `:)`. This opens a shell on port 6200.

### 3. Exploitation
Manually triggered the backdoor using netcat and gained root shell access on the target system.

### 4. Post-Exploitation
- Retrieved `/etc/passwd` — enumerated all system users
- Retrieved `/etc/shadow` — extracted password hashes
- Cracked passwords using John the Ripper — recovered 6 plaintext passwords in seconds

## Key Findings
| Finding | Severity |
|---|---|
| VSFTPd 2.3.4 Backdoor (CVE-2011-2523) | Critical |
| Weak passwords (user=user, msfadmin=msfadmin) | High |
| MD5 password hashing ($1$) | High |
| Multiple unnecessary services exposed | Medium |

## Remediation Recommendations
1. Immediately update VSFTPd to a patched version
2. Enforce strong password policy
3. Replace MD5 hashing with bcrypt or SHA-512
4. Disable unnecessary services (Telnet, FTP) and use SSH instead

## Disclaimer
This lab was conducted in an isolated virtual environment using Metasploitable2, a deliberately vulnerable machine designed for security training. All activities were performed ethically and legally.
