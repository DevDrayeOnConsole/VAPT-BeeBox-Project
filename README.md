# VAPT Course-End Project: Bee-Box (bWAPP) Vulnerability Assessment

**Course:** Advanced Executive Program in Cybersecurity, IIIT Bangalore  
**Module:** Ethical Hacking and VAPT (Vulnerability Assessment & Penetration Testing)  
**Author:** Subham Dey  
**Date:** October 2026

---

## Project Overview

This project presents a controlled vulnerability assessment and ethical hacking exercise against the deliberately vulnerable **Bee-Box (bWAPP)** platform. The engagement follows a five-task methodology: environment setup, vulnerability scanning with Nmap and Nikto, SNMP exploitation using Metasploit, anonymous FTP access verification, and service hardening.

---

## Lab Environment

| Host | Role | IP Address |
|------|------|------------|
| Kali Linux 2025.4 | Attacker / Assessor Workstation | 192.168.140.128 |
| Bee-Box v1.6 | Target (bWAPP, DVWA, Vulnerable Services) | 192.168.140.132 |

---

## Five Tasks Completed

### Task 1: Environment Setup
- Deployed Bee-Box v1.6 in VMware Workstation
- Validated network connectivity via `ping -c 5 192.168.140.132`
- Confirmed target IP: `192.168.140.132/24` (MAC: `00:0c:29:fd:c2:27`)

### Task 2: Vulnerability Scanning
- **Nmap:** Full TCP aggressive scan (`nmap -A -sV -O`) — 17 open TCP ports identified
- **UDP Scan:** SNMP discovered on port 161 (`nmap -sU -p 161`)
- **Nikto:** Web vulnerability scan — 24 findings including critical `#wp-config.php#` credential leak

### Task 3: SNMP Exploitation
- Used Metasploit `auxiliary/scanner/snmp/snmp_enum`
- Default community string `public` granted full MIB access
- Extracted: kernel version, network interfaces, listening ports, process table, filesystem data
- Discovered hidden ports (3632/distccd, 9443) invisible to external TCP scans

### Task 4: Anonymous FTP Access
- Anonymous login successful (code 230) on ProFTPD 1.3.1
- Retrieved complete file listing without authentication
- Ownership metadata (`root:www-data`) exposed

### Task 5: FTP Hardening
- Disabled `<Anonymous ~ftp>` block in `/etc/proftpd/proftpd.conf`
- Restarted ProFTPD service
- Verified hardening: `530 Login incorrect` — anonymous access blocked

---

## Repository Structure

```
VAPT-BeeBox-Project/
├── README.md
├── Reports/
│   └── VAPT_BeeBox_Project_Report.pdf          (Full 17-page report)
├── Source-Code/
│   └── VAPT_BeeBox_Syntax_and_Source_Code.pdf  (All commands & payloads)
├── Executive-Summary/
│   └── VAPT_BeeBox_Executive_Summary.pdf     (2-page overview)
├── Additional-Remarks/
│   └── VAPT_BeeBox_Additional_Comments_Remarks.pdf (8 analytical remarks)
└── Screenshots/
    └── (Lab screenshots — Figures 1-6)
```

---

## Key Findings Summary

| Severity | Count | Examples |
|----------|-------|----------|
| Critical | 3 | Anonymous FTP, SNMP default communities, `#wp-config.php#` leak |
| High | 6 | EOL stack, Samba signing disabled, SSLv2, r-services, root-owned processes |
| Medium | 2 | HTTP TRACE, command injection surface |
| Low | 1 | Directory indexing, missing headers |

**Remediated:** Anonymous FTP (Task 5)  
**Status:** Verified with `530 Login incorrect`

---

## OS Command Injection Coverage

- **Direct Injection:** Demonstrated with `; cat /etc/passwd` payload
- **Blind Injection:** Out-of-band confirmation via `tcpdump` ICMP callback
- **Mitigation:** Input validation, argument-list execution, `escapeshellarg()`, least privilege

---

## Tools Used

- Nmap 7.991 (Network Scanning)
- Nikto 2.5.0 (Web Vulnerability Scanning)
- Metasploit Framework (SNMP Exploitation)
- ProFTPD (FTP Service Hardening)
- Bee-Box v1.6 / bWAPP (Vulnerable Target)

---

## Author

**Subham Dey**  
Cybersecurity Professional | IIIT Bangalore AEP  
[GitHub: DevDrayeOnConsole](https://github.com/DevDrayeOnConsole)
