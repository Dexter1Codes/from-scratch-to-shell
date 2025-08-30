# NETCAT Basics And Manual Banner Grabbing
## installing
To begin, I installed NetCat (OpenBSD variant) using:
```bash
sudo apt install netcat-openbsd
```
I installed this version because this has better functionality and better support.

## 📡 HTTP Request with Netcat
sent a HTTP request and accessed the index.html file of DVWA by using this command:
```bash
echo -e "GET / HTTP/1.1\r\nHost: 127.0.0.1\r\n\r\n" | nc -nv 127.0.0.1 80
```
command breakdown:
- this -e flags the echo to interpret \r, \n characters (newLine characters).
- -nv tells netcat not to run DNS lookups, print useful connection information only.

The output was:
![NetCat HTTP index output](/Day-3-notes/Screenshots/Index-html-file-netcat-output.png)

## Manual Banner Grabbing
The goal was to identify services running on known ports using netcat:
```bash
nc -nv 127.0.0.1 22    # SSH
nc -nv 127.0.0.1 23    # Telnet
nc -nv 127.0.0.1 25    # SMTP
nc -nv 127.0.0.1 110   # POP3
nc -nv 127.0.0.1 143   # IMAP
nc -nv 127.0.0.1 3306  # MySQL
nc -nv 127.0.0.1 8080  # Alternate HTTP
nc -nv 127.0.0.1 443   # HTTPS (SSL-wrapped)
```
Observations:
- All ports responded with Connection Refused, except for port 3306 (MySQL).
- Port 3306 gave a raw MySQL handshake response, indicating that the MySQL service was actively listening.

