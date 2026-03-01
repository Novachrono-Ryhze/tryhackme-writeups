# TryHackMe Writeups – Offensive Security Portfolio

This repository documents my hands-on penetration testing labs completed on TryHackMe.

The purpose of these writeups is to demonstrate structured offensive security methodology, including:

- Service enumeration
- Web application testing
- Credential attacks
- Remote code execution
- Privilege escalation
- Post-exploitation analysis
- Security impact assessment

⚠️ Flags and sensitive lab answers are intentionally omitted.  
These writeups focus on methodology and technical understanding.

---

## 🧠 Methodology

Each machine follows a structured workflow:

1. Reconnaissance
2. Enumeration
3. Initial Access
4. Privilege Escalation
5. Security Impact Analysis
6. Lessons Learned

The goal is not just to solve machines, but to understand *why* vulnerabilities exist and how they can be prevented.

---

## 📂 Completed Machines


| Machine       | Skills Demonstrated | Status |
|--------------|--------------------|--------|
| Lookup       | Web enum, Hydra, Metasploit RCE, SUID analysis, SSH brute force, sudo misconfiguration | Rooted |
| Bricks Heist | WordPress RCE (CVE-2024-25600), REST API exploitation, Reverse shell handling, systemd service analysis, Persistence investigation, Multi-layer encoding (Hex → Base64), Blockchain OSINT, Threat attribution | Rooted |


More machines coming soon.

---

## 🛠 Tools & Technologies

### Recon & Enumeration
- Nmap (Service & version detection)
- Gobuster (Directory brute forcing)
- Manual WordPress enumeration
- REST API inspection

### Exploitation
- curl (Manual HTTP exploitation & JSON payload crafting)
- CVE analysis & public exploit adaptation
- Reverse shell techniques (Bash TCP)
- Netcat (Listener setup & shell handling)

### Post-Exploitation
- Linux process analysis (`ps`, `pgrep`)
- systemd service investigation (`systemctl`)
- Log file discovery & analysis
- File system enumeration
- Privilege escalation fundamentals

### Malware & Persistence Analysis
- Service persistence inspection
- Binary inspection (`file`, `strings`)
- Config/log discovery
- Multi-layer encoding analysis (Hex → Base64 → Base64)

### Scripting & Decoding
- Bash pipelines
- `xxd`
- `base64`
- Pattern searching with `grep`

### Threat Intelligence & OSINT
- Blockchain wallet tracing
- Cryptocurrency transaction analysis
- Threat group attribution research

### Core Technologies
- Linux
- Bash
- Python (basic scripting & automation)

---

## 📌 Disclaimer

All activities documented here were performed in controlled lab environments for educational purposes only.
