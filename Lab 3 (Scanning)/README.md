# Security Assessment Report: Lab 3 - Network Scanning & Enumeration
**Environment:** Decentralized Academic Lab Network (Local Workstation Hosting)

## What We Did
We initiated this phase with a rapid TCP sweep to grab a quick baseline of available services across the local subnet. Because we knew the original machine had configuration issues, we focused our deep enumeration entirely on the functional Metasploitable host running on our teammate's PC. We followed up the sweep by running targeted Nmap Scripting Engine (NSE) scripts against the SMB service to pull down file shares and user accounts.

## Commands & Flags
* `masscan 192.168.100.0/24 -p1-65535 --rate=1000 -oG masscan_results.gnmap`
    * `-p1-65535`: Targets all possible ports.
    * `--rate=1000`: Caps the transmission rate at 1,000 packets per second so we don't crash our local routers.
    * `-oG`: Outputs results in a greppable format for easy parsing.
* `nmap -p 1-65535 -sS -sV --script smb-enum* -oA smb_enum 192.168.100.x`
    * `--script smb-enum*`: Instructs Nmap to run all Lua scripts matching the "smb-enum" wildcard pattern to aggressively enumerate Samba data.
* `enum4linux -a 192.168.100.x`
    * `-a`: Do "All" simple enumeration. This is a macro flag that automatically runs a full suite of checks including: extracting userlists (`-U`), enumerating shares (`-S`), grabbing password policies (`-P`), and checking for OS information (`-o`) via null sessions.

## The Results
We successfully extracted user accounts, underlying RPC data, and open shares from the target's Samba service. By utilizing `enum4linux`, we confirmed the target allowed anonymous null sessions, which dumped the internal user list and password policy. We cross-referenced the discovered software versions and misconfigurations against known CVEs to define our exact paths for exploitation.

### 1. Masscan Subnet Sweep
![Masscan TCP Sweep](./Images/masscan.png)
*Figure 1: High-speed TCP port sweep using masscan to rapidly identify active services across the local subnet.*

---

### 2. Nmap SMB Enumeration
![Nmap SMB Enum Part 1](./Images/nmap_smb_enum1.png)
*Figure 2: Initiation of targeted Nmap SMB enumeration scripts against the Metasploitable host.*

![Nmap SMB Enum Part 2](./Images/nmap_smb_enum2.png)
*Figure 3: Nmap extracting SMB OS discovery and security mode data.*

![Nmap SMB Enum Part 3](./Images/nmap_smb_enum3.png)
*Figure 4: Nmap enumerating open SMB shares and access permissions.*

![Nmap SMB Enum Part 4](./Images/nmap_smb_enum4.png)
*Figure 5: Nmap extracting local user accounts and groups via Samba.*

![Nmap SMB Enum Part 5](./Images/nmap_smb_enum5.png)
*Figure 6: Final Nmap script execution results and scan completion.*

---

### 3. Enum4Linux Null Session & Data Extraction
![Enum4Linux Output](./Images/enum4linux.png)
*Figure 7: Enum4linux successfully executing an anonymous null session to dump the internal userlist, password policy, and deep RPC configurations.*
