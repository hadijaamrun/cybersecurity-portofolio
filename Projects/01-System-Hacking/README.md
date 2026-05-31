# Project 1: System Hacking & Privilege Escalation

## 🎯 Objective
Perform post-exploitation and privilege escalation techniques to gain administrative control over intentionally misconfigured Windows and Linux systems.

## 🛠️ Tools Used
*   Metasploit Framework (MSFvenom)
*   John the Ripper
*   Hashcat
*   `pth-winexe`
*   Netcat

## 🔬 Execution & Methodology

### 1. Windows Privilege Escalation
*   **Insecure Service Executable:** Identified a vulnerable service named `filepermsvc` running with `LocalSystem` privileges. Verification using `accesschk.exe` confirmed that the `Everyone` group had `FILE_ALL_ACCESS` permissions on the service binary.
*   **Exploitation:** Generated a reverse TCP shell payload (`reverse.exe`) using MSFvenom (`msfvenom -p windows/x64/shell_reverse_tcp ...`). The original service binary was overwritten with the payload, and upon restarting the service, a reverse shell with `nt authority\system` privileges was established.
*   **Pass-the-Hash (PtH):** Extracted NTLM hashes from the SAM database. Utilized `pth-winexe` to authenticate directly to the target using the Administrator's NTLM hash (`a9fdfa038c4b75ebc76dc855dd74f0da`) without needing the plaintext password, successfully obtaining a remote command prompt.

### 2. Linux Privilege Escalation
*   **Weak File Permissions:** Discovered that the `/etc/shadow` file was world-readable. Extracted the root hash and successfully cracked it using John the Ripper and the `rockyou.txt` wordlist, revealing the password `password123`.
*   **Sudo Misconfiguration:** Audited `sudo` permissions (`sudo -l`) and found that the user could run `/usr/bin/find` as root without a password. Exploited this via shell escape (`sudo find . -exec /bin/sh -p \; -quit`) to gain a root shell.
*   **SUID Exploitation:** Identified `exim-4.84-3` running with an SUID bit and exploited it using a local root exploit script (CVE-2016-1531).
*   **Information Disclosure:** Found a hidden, world-readable `.ssh` directory at the root level containing a private RSA key (`root_key`). Downloaded the key and used it to SSH into the machine directly as root.

## 🛡️ Mitigation & Recommendations
*   **Least Privilege:** Ensure service binaries and critical scripts are not writable by standard users.
*   **Secure Authentication:** Implement Kerberos instead of NTLM to prevent Pass-the-Hash attacks.
*   **File Permissions:** Restrict access to sensitive files like `/etc/shadow` and SSH private keys using appropriate permissions (e.g., `chmod 600`).
*   **Sudo & SUID Audit:** Regularly audit `sudoers` configurations to prevent shell escape via permitted binaries and remove SUID bits from outdated or vulnerable applications.
