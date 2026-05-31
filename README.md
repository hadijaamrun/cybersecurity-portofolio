# Cybersecurity Portfolio

Welcome to my cybersecurity portfolio! This repository showcases my learning journey, technical skills, projects, certifications, and career aspirations in the field of cybersecurity. It serves as a digital CV to demonstrate my growth and passion for securing critical systems and networks.

## 👤 About Me

Hi! My name is Hanif, and I am a cybersecurity enthusiast dedicated to learning and applying best practices to protect systems and data. Currently, I am a Computer Science student with a focus on penetration testing and threat analysis.

*   **Current Role:** Computer Science Student
*   **Passion:** I have a strong interest in penetration testing, red teaming, and helping companies enhance their security posture by identifying critical vulnerabilities. 
*   **Fun Facts:** When I'm not working on cybersecurity, I enjoy exploring new technologies and expanding my programming knowledge.

---

## 🛠️ Technical Skills

**Tools:**
*   **Network Analysis:** Wireshark, Bettercap
*   **Penetration Testing:** Metasploit, MSFvenom, Burp Suite, Kali Linux, Nmap, Dirb, Gobuster, SQLMap
*   **Malware Analysis:** Tria.ge, Cuckoo Sandbox, Hashcat, John the Ripper
*   **Web Application Security:** OWASP Top 10 Exploitation (SQLi, IDOR, BAC)

**Programming Languages:**
*   JavaScript
*   Bash
*   Python

**Concepts and Frameworks:**
*   **Vulnerability Management:** Identifying, assessing, and mitigating vulnerabilities in systems and networks.
*   **Incident Response Lifecycle:** Detection, identification, containment, eradication, recovery, and lessons learned.
*   **Networking (TCP/IP, Firewalls):** Understanding and securing network protocols and local environments.
*   **Red Team / Blue Team Exercises:** Offensive security practices to simulate and analyze real-world attacks.

---

## 🏆 Certifications and Training

Below are my completed training programs and platforms I actively use to enhance my cybersecurity skills:

**Training Programs:**
*   **Bootcamp Cyber Security Batch 6 - Dibimbing:** Completed intensive training covering network security, web vulnerabilities, system hacking, and malware analysis.

**Learning Platforms:**
*   **Hack The Box (HTB) & TryHackMe (THM):** Active member solving real-world penetration testing challenges and improving hands-on hacking skills.
*   **Burp Suite PortSwigger Academy:** Hands-on practice focused on web application security vulnerabilities.
*   **Cisco Networking Academy:** Learned the basic principles of networking and network defense.

---

## 🚀 Project Experience

Below are five key projects demonstrating my practical skills across different domains of cybersecurity, including network security, web application testing, system hacking, and malware analysis. All exploitations and analyses were strictly conducted within authorized, legally isolated virtual sandbox environments.

### Project 1: System Hacking & Privilege Escalation
**Objective:** Perform post-exploitation and privilege escalation techniques to gain administrative control over Windows and Linux systems[cite: 1].
**Tools Used:** Metasploit, MSFvenom, Hashcat, John the Ripper, `pth-winexe`, Netcat.
**Execution & Outcome:**
*   Exploited an Insecure Service Permissions vulnerability (`filepermsvc`) on a Windows machine by replacing the binary with an MSFvenom reverse shell, granting `nt authority\system` access.
*   Extracted NTLM hashes from the Windows SAM database and successfully authenticated using a Pass-the-Hash attack via `pth-winexe`.
*   On a Linux target, exploited a world-readable `/etc/shadow` file to extract and crack the root password using John the Ripper.
*   Identified a `sudo` misconfiguration on the `/usr/bin/find` binary and executed a shell escape command (`-exec /bin/sh -p`) to gain root privileges.
*   Exploited an SUID vulnerability on `exim-4.84-3` and discovered world-readable SSH root keys to bypass authentication.
*   **Mitigation Recommended:** Implement the Principle of Least Privilege, correct service/file permissions, remove SUID bits from vulnerable binaries, and restrict access to sensitive configuration files.

### Project 2: Web Session Hacking and Hijacking
**Objective:** Execute Man-in-the-Middle (MITM) attacks to intercept network traffic and hijack active web sessions.
**Tools Used:** Bettercap, Wireshark.
**Execution & Outcome:**
*   Conducted network discovery using Bettercap to map the local network and identify the target machine's IP address.
*   Executed ARP Spoofing to successfully intercept communication between the target host and the gateway.
*   Utilized Wireshark to sniff unencrypted HTTP traffic, successfully capturing plaintext credentials and active session cookies.
*   Performed Cookie Injection in the browser to bypass the login portal and hijack the target's session.
*   **Mitigation Recommended:** Enforce HTTPS (SSL/TLS) for all communications, and configure session cookies with "HttpOnly" and "Secure" attributes.

### Project 3: Advanced SQL Injection Attacks
**Objective:** Identify, exploit, and mitigate SQL Injection (SQLi) vulnerabilities in a web application (DVWA).
**Tools Used:** SQLMap, John the Ripper, Burp Suite.
**Execution & Outcome:**
*   Bypassed the application's authentication mechanism by injecting Boolean logic payloads (e.g., `' OR '1'='1`) into input fields.
*   Utilized `UNION SELECT` statements to manually enumerate the database schema and extract metadata, including database names and user privileges.
*   Automated the exploitation process using SQLMap to extract MD5 password hashes from the `users` database table.
*   Successfully cracked the extracted administrator MD5 hashes using John the Ripper and the `rockyou.txt` wordlist.
*   **Mitigation Recommended:** Strongly advised the implementation of Prepared Statements (Parameterized Queries) to completely separate SQL logic from user-supplied data.

### Project 4: Broken Access Control & IDOR Exploitation
**Objective:** Discover hidden resources and exploit authorization flaws to access administrative functions and unauthorized data.
**Tools Used:** Dirb, Gobuster, Burp Suite Intruder.
**Execution & Outcome:**
*   Performed directory enumeration with Dirb and Gobuster to uncover exposed directories like `/admin/` and `/uploads/`.
*   Exploited a Vertical Privilege Escalation vulnerability via Forced Browsing, gaining unauthorized access to the admin panel without administrative credentials.
*   Leveraged Burp Suite Intruder to exploit Insecure Direct Object Reference (IDOR) vulnerabilities by manipulating sequential URL parameters (`id=22`, `id=33`), exposing Personally Identifiable Information (PII) of other users.
*   Exploited a Directory Listing flaw to download an exposed `backup.zip` file, which contained a database backup exposing sensitive data in plaintext JSON format.
*   **Mitigation Recommended:** Implement Server-Side Role Validation, enforce Object-Level Authorization using unpredictable UUIDs instead of sequential IDs, and disable Directory Indexing on the web server.

### Project 5: Malware Analysis in Isolated Environments
**Objective:** Generate, execute, and analyze malware behavior to understand obfuscation limits, system impact, and Command and Control (C2) communication.
**Tools Used:** VirtualBox, MSFvenom, VirusTotal, Tria.ge, Cuckoo Sandbox.
**Execution & Outcome:**
*   Generated a standard Meterpreter reverse shell payload and a heavily obfuscated version using 5 iterations of the polymorphic `x86/shikata_ga_nai` encoder.
*   Conducted static analysis using VirusTotal, revealing that the polymorphic encoder did not reduce detection rates and was easily flagged by modern heuristics.
*   Detonated the payloads within Tria.ge and Cuckoo Sandbox to observe dynamic behavior, including low-level API calls and fileless, in-memory execution.
*   Successfully monitored and logged the malware's attempts to establish a C2 connection to the attacker's IP.
*   **Mitigation Recommended:** Recommended the deployment of behavior-based Endpoint Detection and Response (EDR) systems, Application Whitelisting to block unsigned binaries, and strict Egress Filtering on the firewall.

---

## 📈 Learning Journey

**What I've Learned So Far:**
*   Gained a solid foundation in penetration testing, focusing heavily on OWASP Top 10 vulnerabilities.
*   Learned to use tools like Wireshark, Nmap, and Metasploit for network traffic analysis and threat simulation.
*   Developed skills in privilege escalation (Linux/Windows) and analyzing malware behavior in safe lab environments.

**Improvement Plan:**
*   **Short-term:** Obtain the eJPT (eLearnSecurity Junior Penetration Tester) or PJPT (Practical Junior Penetration Tester) certification in the near future.
*   **Long-term:** Continue advancing my offensive security skills to achieve elite certifications such as OSCP, CEH, or CPTS.

---

## 🎯 Career Objective

My ultimate goal is to work in a specialized offensive security team as a **Penetration Tester** or **Red Teamer**. I aim to specialize in offensive security, helping organizations secure their digital infrastructure by simulating real-world cyber attacks and identifying critical flaws before adversaries do.

---

## 📫 Contact Information

Feel free to reach out to me via the following platforms:
*   **Email:** hanifnurmaajid@outlook.com
*   **LinkedIn:** https://www.linkedin.com/in/hanifanmaj/
*   **GitHub:** https://github.com/hadijaamrun/cybersecurity-portofolio
