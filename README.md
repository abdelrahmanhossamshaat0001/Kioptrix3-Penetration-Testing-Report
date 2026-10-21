# Kioptrix Level 3 — Penetration Testing Report

A complete black-box penetration test of **Kioptrix Level 3 (VM3)**, performed in an isolated VirtualBox lab as part of the **NTI Penetration Testing & Application Security Track — Final Project**.

The assessment demonstrates a full attack chain from network reconnaissance and web enumeration to initial access, credential compromise, privilege escalation, and confirmed root access.

> **Ethical-use notice:** This project was conducted exclusively against an intentionally vulnerable training machine in an isolated, authorized lab environment. The material is provided for educational and defensive-security purposes only.

## Project Overview

| Item | Details |
|---|---|
| Target | Kioptrix Level 3 (VM3) |
| Assessment type | Black-box network and web application penetration test |
| Environment | Isolated VirtualBox host-only lab |
| Objective | Identify vulnerabilities, gain an initial foothold, escalate privileges, and document remediation |
| Final result | Full root compromise confirmed |

## Attack Chain

1. **Reconnaissance** — Nmap host discovery identified the target, followed by a complete TCP service and version scan.
2. **Enumeration** — The web application and its directories were enumerated, revealing Gallarific/LotusCMS components and additional exposed paths.
3. **Initial Access** — An unauthenticated LotusCMS `eval()` code-injection weakness was validated, resulting in command execution and a shell as `www-data`.
4. **SQL Injection** — Injectable gallery parameters were confirmed and used to enumerate the database and recover application credentials.
5. **Credential Access** — Cleartext database credentials and weak unsalted password hashes were identified and independently verified.
6. **SSH Pivot** — Recovered credentials were tested against SSH, providing access to local user accounts.
7. **Privilege Escalation** — A dangerous `NOPASSWD` sudo entitlement for the `ht` editor allowed modification of `/etc/sudoers`.
8. **Root Confirmation** — Unrestricted sudo access was obtained and `whoami` confirmed a root shell.

## Key Findings

| Severity | Finding |
|---|---|
| Critical | SQL Injection in `gallery.php` parameters |
| Critical | Unauthenticated Remote Code Execution in LotusCMS |
| Critical | Dangerous sudo entitlement for `ht` |
| High | Cleartext database credentials in application source |
| High | Weak, unsalted MD5 password hashes |
| High | Credential reuse between the application and operating system |
| Medium | Outdated and end-of-life software stack |
| Medium | Insecure writable web directories |
| Low | Verbose error messages and path disclosure |

## Tools Used

- Nmap
- Gobuster
- Burp Suite / browser-based manual validation
- sqlmap
- Netcat
- phpMyAdmin / MySQL
- Hashcat
- SSH
- HT Editor

## Main Recommendations

- Replace dynamic SQL with parameterized queries and prepared statements.
- Retire or patch the vulnerable CMS and remove `eval()` from user-controlled code paths.
- Remove `NOPASSWD` access to editors, shells, and other privilege-escalation-capable binaries.
- Move database secrets out of application source code and rotate exposed credentials.
- Replace unsalted MD5 with Argon2id, bcrypt, or scrypt.
- Enforce unique credentials across application and operating-system accounts.
- Apply least privilege to application directories and sensitive files.
- Disable verbose production error output and keep software dependencies supported and patched.

## Deliverables

- **Penetration Testing Report:** detailed methodology, evidence, findings, risk ratings, and remediation.
- **Presentation:** concise walkthrough of the attack chain and assessment results.

## Authors

- **Abdulrahman Hossam Shaat**
- **Abdulrahman Wael Nabil**
•Mansour Mohamed
•Mohamed Ali
•Mohamed Farid
## Disclaimer

All IP addresses, credentials, vulnerabilities, and exploitation evidence in this repository relate to an intentionally vulnerable educational machine. Do not test systems without explicit authorization.
