# 🛡️ Group 0 - Ethical Hacking & Offensive Security Practicum

Welcome to the official repository for **Group 0**. This workspace serves as a collaborative archive of penetration testing laboratory exercises, network traffic analysis, and exploit demonstrations completed as part of our Cybersecurity curriculum at Miva Open University.

## 🚀 Repository Overview
This repository contains the artifacts, packet captures (PCAPs), scripts, and documentation generated during our hands-on offensive security labs. All attacks were executed within a safely isolated QEMU/KVM virtualization environment utilizing Kali Linux and intentionally vulnerable target machines.

---

## 🛠️ Technical Stack & Tools
| Category | Tools Utilized |
| :--- | :--- |
| **Recon & Scanning** | Nmap, Masscan, Gobuster, Nikto, Dig, Whois |
| **Exploitation** | Metasploit Framework (MSF), Searchsploit, Sqlmap |
| **Analysis** | Wireshark, Tshark, Tcpdump, Ghidra, Readelf |
| **Wireless/Mobile** | Aircrack-ng, Apktool, Jadx-gui |
| **Custom Scripts** | Python (Scapy, Slowloris), Bash |

---

## 🧪 Lab Modules & Executed Tasks

### Lab 1: Virtual Lab Setup & Configuration
**Targets:** Metasploitable2 & Windows Server (QEMU/KVM Subnet `192.168.100.0/24`)
* **Environment Provisioning:** Deployed core virtual machines and bound adapters to a custom, air-gapped "Isolated" network to ensure zero internet exposure.
* **Connectivity Validation:** Verified network communication via IP configuration tools (`ifconfig`/`ip a`) and ICMP ping tests.
* **Baseline Network Scanning:** Conducted an aggressive, full-port stealth SYN scan (`nmap -sS -p- -T4 -oA baseline_scan`) to map the topology.

### Lab 2: Footprinting & Reconnaissance
* **OSINT Limitations:** Documented the constraints of a strict air-gapped environment. 
* **Active Scanning:** Executed an aggressive 65,535 port scan (`-sV -O`). 
* **Web Vulnerability Scanning:** Targeted the functional web server using Nikto to extract HTTP flaws.

### Lab 3: Network Scanning & Enumeration
* **Rapid Port Sweeping:** Utilized `masscan` for rapid identification of open TCP ports across the subnet.
* **SMB & RPC Enumeration:** Triggered NSE scripts (`--script smb-enum*`) to extract exposed file shares and user account data.

### Lab 4: Exploitation & Post-Exploitation
**Target:** vsftpd v2.3.4 (CVE-2011-2523)
* **Exploitation:** Deployed Metasploit to trigger the known backdoor and bind a shell on TCP Port 6200.
* **Post-Exploitation:** Upgraded to an interactive TTY via Python and gathered system evidence (`uname -a`, `cat /etc/passwd`).
* **Privilege Escalation:** Verified root access via `sudo -l`.

### Lab 5 & 6: Web Application Penetration Testing
**Target:** OWASP Juice Shop
* **Directory Enumeration:** Used `gobuster` to identify hidden Node.js routing patterns.
* **SQL Injection:** Performed manual bypass (`' OR 1=1--`) and automated extraction via `sqlmap` against the backend SQLite database.
* **Cross-Site Scripting (XSS):** Verified client-side vulnerabilities using custom XSS payloads.

### Lab 7: Wireless & Mobile Security
* **WPA2 Cracking:** Executed an offline dictionary attack using `aircrack-ng` and `rockyou.txt` against a captured EAPOL handshake.
* **Mobile Analysis:** Decompiled the DIVA Android application using `apktool` and performed static analysis to find hardcoded API keys.

### Lab 8: Evasion, IDS Bypass & DDoS
* **IDS Evasion:** Implemented packet fragmentation (`nmap --mtu 24`) to bypass signature-based detection.
* **Custom Packet Crafting:** Utilized Python's `Scapy` library to manually forge TCP SYN packets.
* **DDoS Simulation:** Executed a `slowloris` attack against port 80 to demonstrate application-layer denial of service.

---

## Group 0 Team Members

| S/N | Name | Matriculation Number | Contact Email |
| :---: | :--- | :--- | :--- |
| **1** | Ahmed Madugu Zakari | 2024/A/CYB/0080 | ahmed.zakari@miva.edu.ng |
| **2** | Saddiq Musa Adam | 2024/A/CYB/0154 | Saddiq.musa@miva.edu.ng |
| **3** | Jonathan Favour Chisom | 2024/A/CYB/0132 | favour.jonathan@miva.edu.ng |
| **4** | Obioma Felicity Igwe | 2023/B/SENG/0226 | obiomafelicityigwe@gmail.com |
| **5** | Ajayi Oyebanke Abiodun | 2024/C/CYB/0928 | oyebanke.ajayi@miva.edu.ng |
| **6** | Abdulsalam Usman | 2024/A/CYB/0138 | usman.abdulsalam@miva.edu.ng |
| **7** | Oluwadurotimi Bajomo | 2024/C/CYB/0973 | oluwadurotimi.bajomo@miva.edu.ng |
| **8** | Babatunde Kelvin Ifeoluwa | 2023/A/CYB/0055 | kelvin.babatunde@miva.edu.ng |
| **9** | Ikechukwu Francis Nwadialo | 2023/B/CYB/0213 | frankfort4u2020@gmail.com |
| **10** | Joseph Oluwatimilehin Ambali | 2023/B/CYB/0113 | joseph.ambali@miva.edu.ng |
| **11** | Onuawuchi Christian Chinonso | 2023/B/CYB/0206 | christian.onuawuchi@miva.edu.ng |
| **12** | Samuel Ezedi | 2023/B/CYB/0217 | Samuel.ezedi@miva.edu.ng |

---
**⚠️ Legal & Ethical Disclaimer:** *All tools, techniques, and artifacts documented in this repository are for educational purposes only. All attacks were conducted within a closed, authorized laboratory environment. The contributors assume no liability and are not responsible for any misuse or damage caused by this information.*
