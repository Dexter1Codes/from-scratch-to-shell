##  🔢 IP Discovery and Load Balancing
```bash
nmap scanme.nmap.org
```
This command is just for checking if the accessible IP is still active or not. As we can see here, this IP is up.

Now it comes to the NOT SCANNED IPS, these are for load balancing and bypassing traffic if one gets overloaded. The domain name doesn’t just point to a single IP; it maps to multiple servers whose IPs are shown here.

rDNS (reverse DNS) is just the reverse of DNS. Like DNS reflects a domain → IP, rDNS reflects IP → domain:
<details>
<summary>
``` 
server-52-84-206-12.del54.r.cloudfront.net
```
</summary>

**server-52-84-206-12**: just the formatted IP
**del54**: possibly Delhi region (del) + some node code (54)
**r**: not totally sure, maybe "redundancy" or sub-region indicator
**cloudfront.net**: CDN provider by AWS

*CDN* is a service that delivers cached/static data (HTML/images/scripts) to users from physical locations closer to them.
</details>

## 🔢 Nmap Version Scan
```bash
nmap -sV -p 80,443 scanme.nmap.org
```
- These IPs are from Nmap's test targets, not our local machine.

- This checks for services running on specific ports:
    * Port 80 shows Apache httpd 2.4.7
    * Port 443 is filtered, probably due to a firewall or rate-limiting.

- <details> <summary>httpd = HTTP Daemon </summary>
    this tells us that apache is readily available in the background and can be accessed any time, this helps us to understand the logs (records of events happening on system, server, web, application), keeps official records of commonly known vuln and tools outputs.
</details>

If I don’t add -p, Nmap scans the default top 1000 ports.

-p- scans all 65535 ports (takes way longer).


## 🔢 Nmap Aggressive Scan
```bash
nmap -A scanme.nmap.org
```
- <details> <summary> ports 22, 25, and 443 are TCPwrapped </summary>
    TCPwrapped means the service probe is actively rejected (server likely sends a RST (reset) packet instead of SYN_ACK(aknowledgement)), the service closes the connection immediately as soon as the RST packet is reported.
</details>

- Port 80 is open, running apache v2.4.6 on CentOS.

- <details> <summary> SSH host key are compromised at port 22 </summary>
    * Sometimes before the RST packet reaches, some information is leaked, here the SSH host key is leaked through.
    * ```bash
        2048 48:e0:c6:cd:14:00:00:db:b6:b0:3d:f2:0a:2a:3b:6d (RSA)
      ```
        - here 2048 is the bit length of the key.
        - 48:e0:c6...... is the fingerprint (hash of the public key)
        - RSA is the encryption algorithm used by the key
    * There are two more encryption algorithm used, these are ECDSA(Elliptic Curve Digital Signature Algorithm (faster, modern)) and ED25519 (this is a super fast upgrade from RSA with better security).
</details>

- similarly on port 25, it shows SMPT server capabilities retrieved via ```EHLO``` command, these are:
    | Term                  | What It Means                                                                |
    |-----------------------|------------------------------------------------------------------------------|
    | ack.nmap.org          | Just nmap responding                                                         |
    | PIPELINING            | Allows to send multiple SMTP requests without bottling up                    |
    | SIZE 10240000         | Max email size allowed: 10,240,000 bytes (~10 MB)                            |
    | VRFY                  | Server verifies on the receiver end that the user exists                     |
    | ETRN                  | Used to request mail delivery for a specific domain (used in queues)         |
    | STARTTLS              | Server supports upgrading the plain connection to TLS for encryption         |
    | ENHANCEDSTATUSCODES   | Provides more descriptive error/status codes                                 |
    | 8BITMIME              | Supports 8-bit characters in email bodies (non-ASCII)                        |
    | DSN                   | Delivery Status Notification support (like read receipts or failure reports) |

    * <details> <summary> In this port we can also notice the discussion of SSL/TLS </summary>
    there is SSL cert there (fake, expired) was working for 2 days, and SSL is Secure Socket layer, made for securing the communication between clients and servers, what it did was, encrypting traffic (eg. login credentials), verifying the server’s identity using certs, preventing tampering or eavesdropping.
        -TLS (transport Layer Security) is a modern version of SSL, and more advanced, TLS 1.2 is used readily nowadays, TLS 1.3 is the latest and best version till now.
        What everyone calls SSL nowadays is just TLS only.
    </details>

-<details> <summary> This command also detects the OS </summary>
    * A Oracle Virtual Box, with surety of 86%, QEMU (quick emulator) us just a free open-source machine. (This recognition helps us know that the system is virtualized and can be used further for fingerprinting)
</details>

Fingerprinting means identifying details about a system on how it responds to a certain inputs or commands.