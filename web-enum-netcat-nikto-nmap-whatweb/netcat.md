# 🔍 Day 1: Service & Technology Reconnaissance

## 📦 Tool: Netcat (`nc`)

Netcat is a simple but powerful network utility used for banner grabbing, port scanning, and connecting to TCP/UDP services.

### 🔧 Installation

- We chose the more modern version:
```bash
sudo apt install netcat-openbsd
```
- We manually sent an HTTP GET request to the local server on port 80:
```bash
echo -e "GET / HTTP/1.1\r\nHOST: 127.0.0.1\r\n\r\n" | nc -nv 127.0.0.1 80
```

- The response displays the contents of:
`/var/www/html/index.html`

### Manual Banner Grabbing (other services)
```bash
nc -nv 127.0.0.1 22     # SSH
nc -nv 127.0.0.1 23     # Telnet
nc -nv 127.0.0.1 25     # SMTP
nc -nv 127.0.0.1 110    # POP3
nc -nv 127.0.0.1 143    # IMAP
nc -nv 127.0.0.1 3306   # MySQL
nc -nv 127.0.0.1 8080   # Alternate HTTP
nc -nv 127.0.0.1 443    # HTTPS (SSL-wrapped)
```
- Most ports refused connection (Connection Refused), indicating they are **closed** or firewalled.
- The only port that accepted the connection was port 3306, mySQL and in my case mariaDB.
