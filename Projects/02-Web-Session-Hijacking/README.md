# Project 2: Web Session Hacking and Hijacking

## Objective
Execute Man-in-the-Middle (MITM) attacks to intercept local network traffic, extract plaintext credentials, and hijack active web sessions.

## 🛠️ Tools Used
*   Bettercap
*   Wireshark

## Execution & Methodology

### 1. Network Reconnaissance
*   Executed Bettercap (`net.probe on`) to map the local network topology and successfully identified the target victim's IP address and the gateway.

### 2. ARP Spoofing (MITM)
*   Poisoned the target's ARP cache using Bettercap (`set arp.spoof.targets` and `arp.spoof on`) to position the attacker machine between the victim and the gateway, forcing all traffic to flow through the attacker's interface.

### 3. Traffic Sniffing & Credential Harvesting
*   Utilized Wireshark to intercept and analyze the target's network packets.
*   Captured an HTTP `POST` request to `/Login.asp` because the target application was lacking SSL/TLS encryption.
*   Successfully extracted plaintext credentials (`test123/password`) and the active session cookie (`ASPSESSIONIDAQBCATCC=BIEFKHAACL...`) from the intercepted packet.

### 4. Session Hijacking
*   Performed Cookie Injection by modifying the browser's storage data via the developer tools, inserting the stolen `ASPSESSIONID`.
*   Refreshed the page and successfully bypassed the authentication portal, gaining full access to the target's authenticated session without needing to log in.

## Mitigation & Recommendations
*   **Encryption (HTTPS):** Enforce SSL/TLS across the entire web application to encrypt data in transit, making intercepted packets unreadable to attackers.
*   **Secure Cookies:** Implement the `HttpOnly` and `Secure` attributes on session cookies to prevent theft via client-side scripts and ensure cookies are only transmitted over encrypted connections.
*   **Session Management:** Regenerate the Session ID immediately after a successful user login to prevent session fixation attacks.
