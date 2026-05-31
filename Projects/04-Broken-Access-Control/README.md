# Project 4: Broken Access Control & IDOR Exploitation

## 🎯 Objective
Discover hidden web resources and exploit authorization flaws (Broken Access Control and IDOR) to access administrative functions and unauthorized user data.

## 🛠️ Tools Used
*   Dirb
*   Gobuster
*   Burp Suite (Intruder)

## 🔬 Execution & Methodology

### 1. Reconnaissance & Directory Enumeration
*   Executed directory brute-forcing using Dirb and Gobuster with a standard wordlist.
*   Successfully uncovered hidden and unprotected endpoints, returning HTTP 200/301 status codes for `/admin/` and `/uploads/` directories.

### 2. Vertical Privilege Escalation
*   Attempted unauthenticated access to `/admin/panel.php`, which was correctly blocked by session validation.
*   Authenticated as a low-level user and utilized Forced Browsing to manually navigate to `/admin/panel.php`. The application failed to perform Server-Side Authorization Validation, granting full access to the administrative dashboard.

### 3. Horizontal Privilege Escalation (IDOR)
*   Analyzed the user profile endpoint (`profile.php?id=1`) which displayed the current user's data.
*   Utilized Burp Suite Intruder (Sniper attack) to fuzz the `id` parameter with sequential numbers (1-100).
*   Successfully exploited Insecure Direct Object Reference (IDOR) when payloads `id=22` and `id=33` returned HTTP 200 OK responses with differing byte lengths, exposing the Personally Identifiable Information (PII) of other users (Bob and Charlie).

### 4. Information Disclosure
*   Discovered a Directory Listing vulnerability in `/admin/system/` and `/uploads/data/`.
*   Downloaded an exposed `backup.zip` archive containing `database_backup.sql`. Analysis of the file revealed a plaintext JSON database exposing all user records.
*   Retrieved an unprotected internal file named `private-data.txt` marked as confidential.

## 🛡️ Mitigation & Recommendations
*   **Role-Based Access Control (RBAC):** Enforce strict Server-Side Role Validation to ensure users can only access endpoints matching their authorized privilege levels (e.g., `if ($_SESSION['role'] !== 'admin') { block_access(); }`).
*   **Object-Level Authorization:** Validate data ownership before processing requests to prevent IDOR, and replace sequential numeric IDs with unpredictable UUIDs/GUIDs.
*   **Web Server Hardening:** Disable Directory Indexing (e.g., `Options -Indexes` in Apache) and ensure backup files are stored outside the public web root directory.
