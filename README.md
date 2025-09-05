# 🚀 From Scratch to Shell: Offensive Security Learning Notes

This repository documents my **hands-on cybersecurity labs** as I progress through my self-designed roadmap *From Scratch to Shell*.  
The goal: sharpen skills for a **remote cybersecurity internship** and build strong foundations in **penetration testing, vulnerability analysis, and system hardening**.

---

## 📚 What’s Inside

- 🔍 **Practical labs**: DVWA, VulnHub, HackTheBox-style
- 🛠️ **Tool mastery**: Nmap, Burp Suite, Nikto, Netcat, GoBuster, Nuclei
- 💡 **Vulnerability deep dives**: LFI, RFI, SQLi, XSS, Privilege Escalation, Reverse Shells
- 📸 **Screenshots + Notes** for every step

---

## 🗂️ Folder Structure

### 🔎 Web Enumeration
- [Dirb + Burp + Netcat](./web-enum-dirb-burp-netcat/)
- [Dirb + GoBuster + Nuclei](./web-enum-dirb-gobuster-nuclei/)
- [Nmap + WhatWeb + Nikto + Netcat](./web-enum-netcat-nikto-nmap-whatweb/)

### ⚔️ Web Vulnerabilities
- [SQL Injection (DVWA)](./web-vulns-sqli/)
- [Cross-Site Scripting (XSS)](./web-vulns-XSS/)
- [File Inclusion (LFI + RFI)](./file-inclusion-LFI-RFI/)

### 🛡️ Exploitation & Privilege Escalation
- [Privilege Escalation + SUID Enumeration](./priv-esc-reverse-shell/priv-esc-suid.md)
- [Reverse Shell Labs](./priv-esc-reverse-shell/reverse-shell-DVWA.md)

### 🌐 OSINT & Cloud
- [OSINT + Service Fingerprinting](./OSINT-cloud-fingerprinting/)
- [Cloud (OCI) Troubleshooting + Firewall Persistence](./OSINT-cloud-fingerprinting/)
- [API Recon](./api-recon/)

### 🖥️ Infrastructure Troubleshooting
- [Disk Allocation Issue](./Trouble-shooting/Disk-allocation-issue.md)

### 🧪 Miscellaneous

---

## 📅 Current Status

- ✅ Completed: **Web Enumeration + LFI/RFI + SQLi + XSS basics**
- 🔄 In Progress: **Privilege Escalation & Reverse Shells**
- 📍 Next: **OSINT + Cloud Labs**

---

## 🧠 Lessons So Far

- Enumeration is king — most exploits come after proper recon.
- Classic web vulns (SQLi, XSS, LFI) are still alive in real-world apps.
- Misconfigurations are as dangerous as code vulns (e.g., OCI firewall layers).
- Persistence = chaining small missteps into full compromise.

---

## 📜 License
This project is for **educational purposes only**.  
All labs are conducted on intentionally vulnerable environments (DVWA, VulnHub, etc.).  
Do **not** attempt on systems without explicit permission.

