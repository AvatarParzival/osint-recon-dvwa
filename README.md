# OSINT & Reconnaissance Assessment — DVWA

[![Reconnaissance](https://img.shields.io/badge/Domain-OSINT%20%26%20Recon-243B53)](https://en.wikipedia.org/wiki/Open-source_intelligence)
[![DVWA](https://img.shields.io/badge/Target-DVWA-CC0000)](https://dvwa.co.uk/)
[![Kali Linux](https://img.shields.io/badge/Platform-Kali%20Linux-268BEE?logo=kalilinux&logoColor=white)](https://www.kali.org/)
[![Phase](https://img.shields.io/badge/Phase-1%20of%202-F7941E)](https://en.wikipedia.org/wiki/Penetration_test)

A Phase 1 OSINT and reconnaissance assessment performed against a locally deployed instance of the **Damn Vulnerable Web Application (DVWA)**. Eleven information disclosure and configuration findings were identified — two rated High — and a complete attack surface profile was constructed to direct Phase 2 exploitation.

> **Core principle:** every finding is supported by command output or screenshot evidence. No exploitation, privilege escalation, or data modification was performed.

## Final Report

📄 **[Read the complete OSINT & Reconnaissance Assessment Report](OSINT%20and%20Reconnaissance%20Assessment%20Report%20v1.0.pdf)**

- **Version:** v1.0
- **Assessment phase:** Phase 1 of 2 — Reconnaissance
- **Target:** Damn Vulnerable Web Application (DVWA) — `http://127.0.0.1/DVWA/`
- **Assessment date:** 2 August 2026
- **Assessor:** Abdullah Zubair
- **Assessment host:** Kali Linux (hostname: parzival)

## Scope

Passive reconnaissance, technology fingerprinting, service discovery, application enumeration, and information disclosure analysis only. No exploitation or automated attack activity was performed.

## Findings

| ID | Finding | Risk |
|---|---|---|
| OSINT-003 | Exposed Git repository metadata (`/DVWA/.git/`) | 🔴 High |
| OSINT-008 | Exposed backup configuration file (`config.inc.php.bak`) | 🔴 High |
| OSINT-002 | Database service listening on TCP 3306 | 🟠 Medium |
| OSINT-005 | Exposed PHP configuration file (`php.ini`) | 🟠 Medium |
| OSINT-006 | Directory listing enabled on sensitive directories | 🟠 Medium |
| OSINT-009 | Exposed database resource files (SQL scripts & SQLite DBs) | 🟠 Medium |
| OSINT-001 | Missing HTTP security headers | 🟡 Low |
| OSINT-004 | Web server version disclosure | 🟡 Low |
| OSINT-007 | HTML source comment disclosure | 🟡 Low |
| OSINT-010 | JavaScript comment disclosure | 🟡 Low |
| OSINT-011 | Web crawler policy disclosure | ℹ️ Info |

## Screenshots

![MariaDB service status — database listening on TCP 3306](screenshots/Screenshot%202026-08-02%20121849.png)

![DVWA homepage — application accessible and running](screenshots/Screenshot%202026-08-02%20124212.png)

![DVWA setup page — PHP configuration runtime output](screenshots/Screenshot%202026-08-02%20124252.png)

![Apache version and response headers — server version disclosed](screenshots/Screenshot%202026-08-02%20130534.png)

![Nmap scan output — open ports and service versions](screenshots/Screenshot%202026-08-02%20130551.png)

![Directory listing enabled — DVWA file tree exposed](screenshots/Screenshot%202026-08-02%20132754.png)

![PHP configuration file exposed — php.ini accessible](screenshots/Screenshot%202026-08-02%20133331.png)

![Gobuster enumeration — hidden directories and files discovered](screenshots/Screenshot%202026-08-02%20135408.png)

![DVWA config directory listing — sensitive files exposed](screenshots/Screenshot%202026-08-02%20135428.png)

![curl /DVWA/.git/ — Git repository metadata accessible (200 OK)](screenshots/Screenshot%202026-08-02%20135731.png)

![config.inc.php.bak — backup configuration file exposed](screenshots/Screenshot%202026-08-02%20135750.png)

![curl /DVWA/config/ — config directory accessible](screenshots/Screenshot%202026-08-02%20135940.png)

![curl /DVWA/database/ — database resource directory accessible](screenshots/Screenshot%202026-08-02%20140002.png)

![curl HEAD request — server banner and header disclosure](screenshots/Screenshot%202026-08-02%20140028.png)

![HTML source — comment disclosure in page markup](screenshots/Screenshot%202026-08-02%20140400.png)

![JavaScript source — JS files accessible and enumerated](screenshots/Screenshot%202026-08-02%20140419.png)

![JavaScript comment disclosure — internal references in JS](screenshots/Screenshot%202026-08-02%20140436.png)

![JavaScript comment — internal path reference disclosed](screenshots/Screenshot%202026-08-02%20140801.png)

![JavaScript comment — additional internal reference disclosed](screenshots/Screenshot%202026-08-02%20141544.png)

![Apache HTTP Server service start — web server baseline confirmed](screenshots/Evidence%20E-01.png)

## Recommendations

- Remove or restrict access to `.git/` directories on all web servers
- Delete or relocate backup and configuration files outside the web root
- Disable directory listing on all directories (`Options -Indexes`)
- Suppress web server version banners (`ServerTokens Prod`, `ServerSignature Off`)
- Implement missing HTTP security headers: `Content-Security-Policy`, `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy`
- Restrict access to `php.ini`, `config.inc.php.bak`, and all database resource files
- Remove internal paths and developer references from HTML and JavaScript comments

## Repository Structure

```text
.
├── README.md
├── OSINT and Reconnaissance Assessment Report v1.0.pdf
└── screenshots/
    ├── Evidence E-01.png
    ├── Screenshot 2026-08-02 121849.png
    ├── Screenshot 2026-08-02 124212.png
    ├── Screenshot 2026-08-02 124252.png
    ├── Screenshot 2026-08-02 130534.png
    ├── Screenshot 2026-08-02 130551.png
    ├── Screenshot 2026-08-02 132754.png
    ├── Screenshot 2026-08-02 133331.png
    ├── Screenshot 2026-08-02 135408.png
    ├── Screenshot 2026-08-02 135428.png
    ├── Screenshot 2026-08-02 135731.png
    ├── Screenshot 2026-08-02 135750.png
    ├── Screenshot 2026-08-02 135940.png
    ├── Screenshot 2026-08-02 140002.png
    ├── Screenshot 2026-08-02 140028.png
    ├── Screenshot 2026-08-02 140400.png
    ├── Screenshot 2026-08-02 140419.png
    ├── Screenshot 2026-08-02 140436.png
    ├── Screenshot 2026-08-02 140801.png
    └── Screenshot 2026-08-02 141544.png
```

## Limitations

- Assessment was conducted against a locally deployed DVWA instance in an isolated lab — findings do not reflect a production environment.
- Activity was restricted to Phase 1 passive reconnaissance and non-exploitative enumeration only.
- Phase 2 exploitation is out of scope for this repository.

## Lessons Learned

- Information disclosure rarely requires exploitation — enumeration alone can reveal credentials, source code, and database structure.
- A `.git/` directory left in the web root allows full source reconstruction without touching the application.
- Backup files are often forgotten and never rotated out of the web root; they carry the same sensitivity as the originals.
- Missing security headers are low-effort to add and meaningfully raise the cost of follow-on attacks.

## Author

**Abdullah Zubair**  
Cybersecurity | GRC | Security Automation
- GitHub: [@AvatarParzival](https://github.com/AvatarParzival)
- LinkedIn: [Abdullah Zubair](https://www.linkedin.com/in/abdullahzubairr)
- Email: [abdullah69zubair@gmail.com](abdullah69zubair@gmail.com)

## Responsible Use

This repository is intended for educational, defensive-security and professional portfolio purposes. All testing was performed against an intentionally vulnerable application in an isolated lab. Do not reproduce these techniques against systems without explicit written authorisation.
