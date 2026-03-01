# Lookup – TryHackMe Writeup

## Objective

Obtain initial access to the target system and escalate privileges to root.

---

## Overview

Lookup is a beginner-level machine that demonstrates the complete offensive security workflow:

- Service enumeration
- Virtual host discovery
- Credential attacks
- Remote code execution
- SUID binary analysis
- Credential harvesting
- SSH lateral movement
- Sudo misconfiguration exploitation

This writeup documents the methodology used, focusing on understanding rather than tool dependency.

---

## Tools Used

- Nmap
- Gobuster
- Hydra
- Metasploit
- SSH
- Linux enumeration techniques
- Custom wordlists

---

## Reconnaissance

An initial Nmap scan revealed:

- **Port 22** – SSH
- **Port 80** – HTTP

The web application required proper virtual host configuration.  
After modifying `/etc/hosts`, the website loaded correctly.

This highlights the importance of checking for virtual host–based routing during enumeration.

---

## Web Enumeration

The login page responded differently depending on whether a username was valid.

By analyzing the error messages, it was possible to perform **username enumeration** and identify valid users.

Once valid usernames were discovered, a credential attack was performed against the login form.

Using Hydra, a weak password was successfully identified.

This provided authenticated access to the web application.

---

## Subdomain Discovery

After authentication, a new subdomain was identified:
**files.lookup.thm**

Accessing this subdomain revealed an instance of **elFinder**, a web-based file manager.

This significantly expanded the attack surface.

---

## Exploitation – elFinder Command Injection

The elFinder instance was vulnerable to command injection via its PHP connector.

Using a Metasploit module targeting the elFinder `exiftran` vulnerability:

- A malicious payload was uploaded
- Command injection was triggered during image processing
- A reverse shell was obtained

Initial access was achieved as:
**www-data**


This demonstrates how improper input sanitization in file management systems can lead to remote code execution.

---

## Privilege Escalation – User Access

After gaining a foothold, local enumeration was performed.

A custom SUID binary was discovered:
**/usr/sbin/pwm**

Using `strings`, it was observed that the binary executed the `id` command via `popen()`.

This opened the possibility of **PATH hijacking**.

By manipulating the PATH environment variable and creating a malicious replacement for `id`, it was possible to extract a password list belonging to a local user.

A wordlist was constructed from the extracted passwords.

Hydra was then used to brute-force SSH access for the user account:
**think**


SSH login was successfully obtained.

---

## Privilege Escalation – Root

Running:
**sudo -l**

revealed that the user `think` was allowed to execute: 
**/usr/bin/look**


as root.

The `look` utility prints lines from a file that begin with a given string.

Because it was allowed to run as root, it could be abused to read restricted files.

By using:
**sudo /usr/bin/look "" /root/root.txt**


the root flag was retrieved.

This demonstrates how allowing seemingly harmless binaries in sudo configurations can lead to full system compromise.

---

## Attack Chain Summary

1. Enumerated services (Nmap)
2. Identified valid usernames via response analysis
3. Brute-forced web login credentials
4. Discovered subdomain
5. Exploited elFinder RCE
6. Gained reverse shell as www-data
7. Identified custom SUID binary
8. Performed PATH hijacking to extract passwords
9. Brute-forced SSH user access
10. Abused sudo misconfiguration to read root files

---

## Security Impact

This machine demonstrates:

- Weak credential management enables lateral movement
- Custom SUID binaries can introduce critical vulnerabilities
- PATH manipulation can lead to privilege escalation
- Misconfigured sudo permissions can allow arbitrary file access
- Small web vulnerabilities can escalate into full system compromise

---

## Lessons Learned

- Always check for virtual host configurations
- Analyze SUID binaries carefully
- Inspect binaries with `strings` and `file`
- Misconfigured sudo rules are high-value targets
- Structured methodology is more important than tools

---

## Final Notes

Flags and sensitive lab answers have been intentionally omitted.

This writeup focuses on demonstrating understanding of the attack process and privilege escalation techniques.

