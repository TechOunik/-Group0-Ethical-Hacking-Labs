# 🛡️ Group 0 - Ethical Hacking & Offensive Security Practicum

Welcome to the official repository for **Group 0**. This workspace serves as a collaborative archive of penetration testing laboratory exercises, network traffic analysis, and exploit demonstrations completed as part of our Cybersecurity curriculum at Miva Open University.

## 🎯 Repository Overview
This repository contains the artifacts, packet captures (PCAPs), scripts, and documentation generated during our hands-on offensive security labs. All attacks were executed within a safely isolated QEMU/KVM virtualization environment utilizing Kali Linux and intentionally vulnerable target machines.

---

## 📂 Lab Modules & Executed Tasks

### Lab 1: Virtual Lab Setup & Configuration
**Targets:** Metasploitable2 & Windows Server (QEMU/KVM Subnet `192.168.100.0/24`)
* **Environment Provisioning:** Deployed core virtual machines and bound adapters to a custom, air-gapped "Isolated" network to ensure zero internet exposure.
* **Connectivity Validation:** Verified network communication via IP configuration tools (`ifconfig`/`ip a`) and ICMP ping tests from the Kali attacker machine.
* **Baseline Network Scanning:** Conducted an aggressive, full-port stealth SYN scan (`nmap -sS -p- -T4 -oA baseline_scan 192.168.100.0/24`) to map the topology and establish a clean traffic baseline.

### Lab 2: Footprinting & Reconnaissance
**Targets:** Air-gapped Subnet & Team Member's Working Metasploitable VM
* **OSINT Limitations:** Attempted passive OSINT (`whois`, `dig`) but successfully documented their failure due to the strict air-gapped environment. Researched Shodan filter syntax (`port:`, `os:`) as a theoretical alternative.
* **Active Scanning:** Executed an aggressive 65,535 port scan (`nmap -sS -sV -O -p1-65535 -T4`). Discovered a closed HTTP Port 80 on the primary VM and strategically pivoted to a team member’s verified environment.
* **Web Vulnerability Scanning:** Targeted the functional web server using Nikto (`nikto -h http://<target_IP> -output nikto_report.txt`) to extract known HTTP flaws.
* **Network Visualization:** Consolidated intelligence to generate an accurate visual map of the network environment.

### Lab 3: Network Scanning & Enumeration
**Targets:** Functional Metasploitable VM
* **Rapid Port Sweeping:** Utilized `masscan <target_subnet> -p1-65535 --rate=1000 -oG masscan_results.gnmap` for rapid identification of open TCP ports.
* **Deep Service Enumeration:** Profiled active services using Nmap's service versioning (`-sV`) against all 65,535 ports.
* **SMB & RPC Enumeration:** Triggered specific Nmap Scripting Engine (NSE) scripts (`nmap -p 1-65535 -sS -sV --script smb-enum* -oA smb_enum <target_IP>`) to extract exposed file shares and user account data from the Samba service.

### Lab 4: Exploitation & Post-Exploitation
**Target:** vsftpd v2.3.4 (CVE-2011-2523)
* **Vulnerability Research:** Used `searchsploit` to confirm the presence of a known malicious backdoor in vsftpd 2.3.4.
* **Exploitation:** Deployed the Metasploit Framework to trigger the backdoor (sending a `:)` string) and bind a shell on TCP Port 6200.
* **Post-Exploitation:** Upgraded the initial "dumb" shell to an interactive TTY using Python (`python -c 'import pty; pty.spawn("/bin/bash")'`). Gathered system evidence via `uname -a` and `cat /etc/passwd`.
* **Privilege Escalation:** Verified automatic highest-level system privileges using `sudo -l`.
* **Remediation Planning:** Authored a defense plan recommending immediate software patching, migration to secure protocols (SFTP), strict `iptables` rules, and deployment of an IDS (Snort/Suricata).

### Lab 5 & 6: Web Application Penetration Testing
**Target:** OWASP Juice Shop (Localhost `127.0.0.1`)
* **Directory Enumeration:** Deployed `gobuster`, identifying how modern Node.js/Single-Page Applications handle static routing versus traditional web servers.
* **Manual SQL Injection:** Successfully bypassed the administrator login utilizing the `' OR 1=1--` payload, exploiting order-of-operations logic in the backend database.
* **Automated SQL Injection:** Mapped the hidden backend API (`/rest/products/search`) and deployed `sqlmap` with aggressive, targeted parameters (`--dbms=sqlite --level=5 --risk=3 --dump`) to bypass filters and extract the local SQLite database.
* **Cross-Site Scripting (XSS):** Executed client-side XSS payloads to trigger alert boxes on the target application.

### Lab 7: Wireless & Mobile Security
**Targets:** DIVA Application (Vulnerable APK) & standard WPA2 EAPOL Capture
* **WPA2 Handshake Cracking:** Downloaded an open-source WPA2 `.cap` file and executed an offline dictionary attack using `aircrack-ng` and the `rockyou.txt` wordlist to recover the pre-shared key. 
* **APK Reverse Engineering:** Utilized `apktool` to decompile the DIVA Android application into readable source directories.
* **Static Analysis:** Conducted command-line enumeration using `grep` to hunt for and extract hardcoded developer passwords and API keys hidden within `res/values/strings.xml`.

### Lab 8: Evasion, IDS/Firewall Bypass & DDoS Simulation
**Target:** Metasploitable VM (Custom `192.168.100.x` QEMU/KVM interface `eth0`)
* **Packet Capture:** Used `tcpdump -i eth0` to record all attack traffic, later analyzing the binary `.pcap` files using `tshark` and Wireshark for human-readable evidence.
* **IDS Evasion:** Executed packet fragmentation techniques using `nmap --mtu 24` to obscure the scan signature from potential firewalls.
* **Custom Packet Crafting:** Utilized Python's `scapy` library to manually forge and transmit custom TCP SYN packets directly to the target.
* **DDoS Simulation:** Deployed `slowloris` against the target's port 80, holding open HTTP connections to simulate a low-impact, application-layer Denial of Service attack.

---

## 👥 Group 0 Team Members

| S/N | Name | Matriculation Number | Miva Email |
| :---: | :--- | :--- | :--- |
| **1** | Ahmed Madugu Zakari | 2024/A/CYB/0080 | ahmed.zakari@miva.edu.ng |
| **2** | Saddiq Musa Adam | 2024/A/CYB/0154 | Saddiq.musa@miva.edu.ng |
| **3** | Jonathan Favour Chisom | 2024/A/CYB/0132 | favour.jonathan@miva.edu.ng |
| **4** | Obioma Felicity Igwe | 2023/B/SENG/0226 | obioma.igwe@miva.edu.ng |
| **5** | Ajayi Oyebanke Abiodun | 2024/C/CYB/0928 | oyebanke.ajayi@miva.edu.ng |
| **6** | Abdulsalam Usman | 2024/A/CYB/0138 | usman.abdulsalam@miva.edu.ng |
| **7** | Oluwadurotimi Bajomo | 2024/C/CYB/0973 | oluwadurotimi.bajomo@miva.edu.ng |
| **8** | Babatunde Kelvin Ifeoluwa | 2023/A/CYB/0055 | kelvin.babatunde@miva.edu.ng |
| **9** | Ikechukwu Francis Nwadialo | 2023/B/CYB/0213 | ikechukwu.nwadialo@miva.edu.ng |
| **10** | Joseph Oluwatimilehin Ambali | 2023/B/CYB/0113 | joseph.ambali@miva.edu.ng |
| **11** | Onuawuchi Christian Chinonso | 2023/B/CYB/0206 | christian.onuawuchi@miva.edu.ng |
| **12** | Samuel Ezedi | 2023/B/CYB/0217 | Samuel.ezedi@miva.edu.ng |

---
**⚠️ Legal & Ethical Disclaimer:** *All tools, techniques, and artifacts documented in this repository are for educational purposes only. All attacks were conducted within a closed, authorized laboratory environment. The contributors assume no liability and are not responsible for any misuse or damage caused by this information.*
