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
*   **Exploitation:** Generated a reverse TCP shell payload (`reverse.exe`) using MSFvenom. The original service binary was overwritten with the malicious payload. Upon restarting the service, a reverse shell with `nt authority\system` privileges was successfully established.
*   **Pass-the-Hash (PtH):** Extracted NTLM hashes from the Windows SAM database. Utilized `pth-winexe` to authenticate directly to the target using the Administrator's NTLM hash (`a9fdfa038c4b75ebc76dc855dd74f0da`) without requiring the plaintext password, successfully obtaining an interactive remote command prompt.

### 2. Linux Privilege Escalation
*   **Weak File Permissions:** Discovered that the `/etc/shadow` file was world-readable (`-rw-r--r--`). Extracted the root password hash and successfully cracked it offline using John the Ripper and the `rockyou.txt` wordlist, revealing the password `password123`.
*   **Sudo Misconfiguration:** Audited `sudo` permissions (`sudo -l`) and found that the standard user was allowed to run the `/usr/bin/find` binary as root without a password. Exploited this vulnerability using a shell escape technique (`sudo find . -exec /bin/sh -p \; -quit`) to drop into a root shell.
*   **SUID Exploitation:** Identified `exim-4.84-3` running with an SUID bit set and exploited it using a local root exploit script (`cve-2016-1531.sh`) to gain elevated access.
*   **Information Disclosure:** Enumerated the root directory (`ls -la /`) and found a hidden `/.ssh` directory. Inside, a private RSA key (`root_key`) was found with world-readable permissions (`-rw-r--r--`). Copied the key to the attacker machine, modified the permissions to `chmod 600`, and used it to SSH directly into the target as root (`ssh -i root_key ... root@10.48.178.32`) without password interaction.

## 🛡️ Mitigation & Recommendations
*   **Least Privilege:** Ensure that system service binaries and execution scripts cannot be modified or written to by standard users.
*   **Secure Authentication:** Transition to the Kerberos protocol instead of NTLM to mitigate the risk of Pass-the-Hash attacks.
*   **File Permissions Management:** Restrict access to sensitive files like `/etc/shadow` and SSH private keys by enforcing strict permissions (e.g., `chmod 600`).
*   **Sudo & SUID Audit:** Regularly audit `sudoers` configurations to prevent shell escapes via legitimate binaries (such as `find`, `vim`, or `awk`), and remove SUID bits from outdated applications or those that do not require such privileges.
