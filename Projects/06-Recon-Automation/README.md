# Recon Automation

**Repository:** [https://github.com/hadijaamrun/recon-automation-hanif](https://github.com/hadijaamrun/recon-automation-hanif)

## 1. Objective
The primary objective of this project is to automate the end-to-end reconnaissance process using a robust and efficient Bash script. This script integrates three main recon tools into a seamless pipeline to automatically discover subdomains and identify live hosts without manual intervention or errors.

## 2. Tools Used
* **Go Environment:** The foundational framework required to install, compile, and run Go-based tools.
* **PDTM (ProjectDiscovery Tools Manager):** Used to easily install and manage ProjectDiscovery tools.
* **Subfinder:** A fast, passive subdomain discovery tool used to gather valid subdomains from target domains.
* **HTTPX:** A fast and multi-purpose HTTP toolkit used to probe discovered subdomains for active hosts, extracting HTTP status codes and web page titles.
* **Anew:** A utility by tomnomnom used to append new lines to files, effectively filtering out duplicate entries so the final output remains clean.

## 3. Execution & Methodology
* **Clone the Repository:** First, clone the project to your local machine using:
  `git clone https://github.com/hadijaamrun/recon-automation-hanif.git`
  `cd recon-automation-hanif`
* **Preparation & Input:** Target domains are added to the `input/domains.txt` file (minimum of 5 targets). Execution permission is granted to the script using the command `chmod +x scripts/recon-auto.sh`, and it is then executed from the root project directory.
* **Validation & Setup:** Upon execution, a unique timestamp is generated to dynamically name the output and log files. The script verifies that all required tools are installed and that the input file exists. If any component is missing, the script halts automatically (`exit 1`) to prevent further errors.
* **Subdomain Enumeration:** The script reads the target domains line by line. **Subfinder** searches for subdomains, redirects any errors to `errors.log`, and pipes the results directly into **Anew** to ensure only unique subdomains are saved.
* **Live Host Identification:** The collected unique subdomains are then processed by **HTTPX**, which is configured with 100 parallel threads (`-t 100`) for maximum speed. HTTPX checks for active hosts and captures the HTTP status code (`-sc`) and page title (`-title`). The results are piped through **Anew** once more to ensure the final list of live hosts is clean and deduplicated.
* **Continuous Logging System:** A custom logging function uses a combination of `echo` and `tee -a` to display progress on the terminal with timestamps, while simultaneously saving the output to a permanent log file.

## 4. Conclusion
Automated pipeline for the initial reconnaissance phase in penetration testing or bug bounty hunting. By chaining Subfinder, HTTPX, and Anew, this project eliminates tedious manual processes, prevents data duplication, handles errors gracefully in the background, and produces a clean, actionable list of live targets complete with URLs, HTTP status codes, and web page titles ready for further analysis.
