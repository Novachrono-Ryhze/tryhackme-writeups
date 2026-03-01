# Lookup – TryHackMe Writeup

## Overview
Lookup is a beginner-level machine that demonstrates:
- Web enumeration
- Credential attacks
- Remote code execution
- Privilege escalation

---

## Reconnaissance

Initial scan revealed:
- Port 22 (SSH)
- Port 80 (HTTP)

Virtual host configuration required modification of /etc/hosts.

---

## Initial Access

- Performed username enumeration
- Discovered valid users
- Brute-forced web login
- Identified subdomain: files.lookup.thm
- Detected elFinder file manager

---

## Exploitation

Used Metasploit module targeting elFinder command injection vulnerability.

Successfully obtained reverse shell as:
**www-data**

---

## Privilege Escalation (User)

- Enumerated SUID binaries
- Identified custom binary
- Analyzed using strings
- Extracted password list
- Brute-forced SSH for user account

Gained access as:
**think**

---

## Privilege Escalation (Root)

- Discovered sudo misconfiguration
- User allowed to execute `/usr/bin/look`
- Abused it to read root files

---

## Lessons Learned

- Importance of virtual host enumeration
- Risks of custom SUID binaries
- Misconfigured sudo entries can lead to full compromise
- Structured attack methodology is critical
