#### ApexPlanet Cybersecurity & Ethical Hacking Internship — Task 2

#### Network Security & Vulnerability Scanning

**Intern:** Firdous Jahan Begum
**Organization:** ApexPlanet Software Private Limited
**Lab Environment:** Kali Linux Rolling (WSL2 on Windows)
**Authorized Targets:** scanme.nmap.org (45.33.32.156) & Local Loopback (127.0.0.1) 

#### Repository Contents

* **Task2_Network_Security_Report.pdf** — Complete technical report with embedded terminal screenshots, CVE severity matrix, packet capture analysis, and remediation steps.
* **README.md** — Structured scan analysis, reconnaissance methodology, vulnerability summary, and defense documentation.

#### 1. Passive & Active Reconnaissance

* **Passive OSINT & DNS Enumeration:** Used nslookup scanme.nmap.org to map IPv4 (45.33.32.156) and IPv6 (2600:3c01::f03c:91ff:fe18:bb2f) addresses without sending probe packets to target ports, alongside WHOIS, Google Dorking, and Shodan reconnaissance concepts.
* **Active Host Discovery & Banner Grabbing:** Executed nmap -sn 127.0.0.0/30 to verify live hosts and curl -I to extract HTTP server banners (Apache/2.4.6 (CentOS)).

#### 2. Nmap Port, Service & OS Enumeration

* **Command:** sudo nmap -sS -sV -O -Pn --top-ports 20 scanme.nmap.org and sudo nmap -sU -Pn --top-ports 5 scanme.nmap.org
* **Key Findings:** 

  * 22/tcp — **Open** (OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13)
  * 80/tcp — **Open** (Apache httpd 2.4.7 ((Ubuntu)))
  * 25/tcp — **Filtered** (Blocked by upstream firewall)
  * 123/udp — **Open** (NTP)
  * **OS Fingerprint:** Linux Kernel 4.19 – 5.15 (98% confidence)

#### 3. Vulnerability Assessment & Remediation Summary

SeverityCVE / FindingServiceRemediation
****Critical (9.8)****
CVE-2023-38408 (ssh-agent RCE)OpenSSH 6.6.1p1Upgrade OpenSSH to v9.3p2+ and disable agent forwarding
****Critical (9.8)****
CVE-2016-1908 (X11 Auth Bypass)OpenSSH 6.6.1p1Patch OpenSSH and set X11Forwarding no
****High (8.5)****
CVE-2015-5600 (MaxAuthTries Bypass)OpenSSH 6.6.1p1Enforce SSH key authentication and Fail2Ban
****Medium (7.5)****
Outdated Web Server / Slowloris RiskApache 2.4.7Upgrade Apache to 2.4.58+ and enable mod_reqtimeout
****Low (Info)****
Publicly Reachable NTP ServiceUDP/123Restrict NTP access via firewall to trusted peers

#### 4. Packet Analysis & Firewall Hardening

* **Traffic Inspection (tshark + hping3):** Captured simulated cleartext protocol traffic and a controlled local TCP SYN flood (hping3 -S -p 8081 --flood), demonstrating how cleartext protocols expose plaintext credentials and how SYN floods generate high-rate Win=512 [SYN] packets.
* **Host Firewall Mitigation (iptables):** Configured iptables -A INPUT -p tcp --dport 8080 -j DROP and verified with Nmap that port 8080/tcp changed to the filtered state.
