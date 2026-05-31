# Project 3: Advanced SQL Injection Attacks

## 🎯 Objective
Identify, exploit, and mitigate SQL Injection (SQLi) vulnerabilities in a web application (DVWA) to bypass authentication and extract sensitive database records.

## 🛠️ Tools Used
*   SQLMap
*   John the Ripper
*   Burp Suite

## 🔬 Execution & Methodology

### 1. Vulnerability Identification & Auth Bypass
*   Injected a single quote (`'`) into the User ID parameter, which triggered a MariaDB syntax error, confirming the absence of input sanitization.
*   Successfully bypassed the authentication mechanism by injecting a Boolean logic payload (`' OR '1'='1`), which forced the database to evaluate the condition as true and return administrative data.

### 2. Manual Database Enumeration
*   Utilized `ORDER BY` clauses to determine the number of columns in the original query.
*   Executed `UNION SELECT` statements to extract the current user (`app@localhost`), database name (`dvwa`), and database version.
*   Extracted table names from `information_schema.tables`, confirming the existence of a sensitive `users` table.

### 3. Automated Data Exfiltration
*   Deployed SQLMap (`sqlmap -u ... --dbs`) to automate the extraction process and mapped the entire database structure.
*   Successfully dumped the `users` table, obtaining sensitive user metadata including usernames, avatars, and MD5 password hashes.

### 4. Password Cracking
*   Saved the administrator's MD5 hash (`5f4dcc3b5aa765d61d8327deb882cf99`) into a text file.
*   Utilized John the Ripper (`--format=raw-md5 --wordlist=rockyou.txt`) to crack the hash offline, successfully retrieving the plaintext password (`password`) in seconds due to the lack of cryptographic salt.

## 🛡️ Mitigation & Recommendations
*   **Secure Coding:** Strictly implement Prepared Statements (Parameterized Queries) to separate SQL instructions from user-supplied data, neutralizing injection attempts.
*   **Least Privilege:** Configure database user accounts with minimal necessary privileges, restricting access to system tables like `information_schema`.
*   **Modern Hashing:** Replace outdated MD5 hashing algorithms with robust, computationally expensive alternatives like BCrypt or Argon2, and always include a unique salt.
