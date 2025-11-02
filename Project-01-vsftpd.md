# Project 1: Metasploitable2 vsftpd 2.3.4 Backdoor Exploit

### Summary
Conducted a penetration test against a vulnerable Linux server (Metasploitable2) within an isolated VirtualBox environment. I successfully identified and exploited a known backdoor in the vsftpd service to gain root-level access.

### Tools Used
* **Virtualization:** Oracle VirtualBox
* **Attacker VM:** Kali Linux
* **Victim VM:** Metasploitable2
* **Framework:** Metasploit
* **Scanner:** Nmap

### Methodology

**1. Reconnaissance**
* Established a VirtualBox "NAT" network (10.0.2.0/24) for the attacker and victim VMs.
* Identified the target IP (`10.0.2.3`) using the `ifconfig` command.
* Ran an `nmap` scan from within `msfconsole` (`db_nmap -v -sS -A 10.0.2.3`) to enumerate services.

**2. Vulnerability Analysis**
* The `nmap` scan revealed that port 21/tcp was open and running `vsftpd 2.3.4`.
* Research confirmed this specific version contains a well-known backdoor (CVE-2011-2523) that grants a remote attacker a command shell.

**3. Exploitation**
* Using Metasploit, I loaded the `exploit/unix/ftp/vsftpd_234_backdoor` module.
* Set `RHOSTS` to `10.0.2.3` and executed the `run` command.
* The exploit was successful, opening a "Command shell session 1".

**4. Post-Exploitation**
* Confirmed administrative access by running the `whoami` command, which returned `root`.
* This level of access grants complete control over the victim machine.

**5. Remediation**
* **Immediate Fix:** The `vsftpd` service should be immediately upgraded to a patched version (2.3.5 or later).
* **Compensating Control:** If an upgrade is not possible, the FTP service should be shut down, disabled, and firewalled.
