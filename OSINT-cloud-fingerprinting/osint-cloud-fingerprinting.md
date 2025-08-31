# OSINT & Cloud Service Fingerprinting (Oracle Cloud VM)
> Note: *All IP addresses in this report are anonymized for security reasons*
## Shadon
- My Oracle VM (140.245.8.253) was not indexed, as expected for new cloud hosts.
- Testing on scanme.nmap.org (45.33.32.156) demonstrated rich banner and port enumeration data, proving the method.
**Web Tech used:**
![Shadon scanme.nmap.org web tech output](/OSINT-cloud-fingerprinting/Screenshots/Shadon-1.png)
**Open ports:**
![Shadon scanme.nmap.org open ports ouput](/OSINT-cloud-fingerprinting/Screenshots/Shadon-2.png)
**Common Vulns:**
![Shadon scanme.nmap.org common vulns output](/OSINT-cloud-fingerprinting/Screenshots/Shadon-3.png)

## Overview
This lab focused on applying **OSINT techniques** and **service fingerprinting** to a self-hosted Oracle Cloud VM.  
While the intent was to enumerate services (WHOIS, DNS, Shodan, theHarvester), a significant part of the exercise turned into debugging **multi-layered networking issues** — an invaluable real-world lesson.

---

## Reconnaissance

### WHOIS
```bash
whois 140.245.8.253
```
result:
![oracle who-is Output](/OSINT-cloud-fingerprinting/Screenshots/oracle-whois-output.png)
- Revealed registration metadata for the Oracle-assigned IP block.

### Reverse DNS lookup
```bash
dig -x 140.245.8.253
```
result:
![dig output](/OSINT-cloud-fingerprinting/Screenshots/dig-output.png)
takeaways:
- NXDOMAIN → no PTR record exists.
- Flags observed:
    1. qr (query response)
    2. rd (recursion desired)
    3. ra (recursion available)

### Nmap (VM External Scan)
commands used:
```bash
nmap -p 80,22 140.245.8.253
nmap -sV 140.245.8.253
```
result:
![nmap service scan](/OSINT-cloud-fingerprinting/Screenshots/nmap-version-scan.png)
![nmap port scan](/OSINT-cloud-fingerprinting/Screenshots/nmap-port-scan.png)
- All ports filtered
- The first possible cause i found was the NSGs were incompatible with the ports.

## Debugging 

- Created some custom ingress rules in NSG:
    1. 22/tcp (SSH)
    2. 80/tcp (HTTP)
    both TCP and `source:0.0.0.0/0`
- what i observed:
    1. SSH was discoverable
    2. HTTP still filtered
- checked apache2 locally on oracle VM:
``` bash
curl -I http://127.0.0.1   # Worked
curl -I http://10.0.0.79   # Worked (private VNIC IP)
curl -I http://140.245.8.253 # Failed (public IP)
```
    Confirmed that Apache functional internally, blocked externally.

### Tcpdump Validation
```bash
sudo tcpdump -nni ens3 'tcp port 80'
```
Observed inbound SYN packets from my client, but no proper response.
Suggested host firewall / iptables was blocking.

I tried checking for the firewalls:
```bash
sudo ufw status
```
 and got to know that the firewall was inactive.

 ### IPtables
 Applied some baseline rules:
 ```bash
 # Allow existing connections
sudo iptables -I INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow loopback
sudo iptables -I INPUT -i lo -j ACCEPT

# Allow SSH + HTTP
sudo iptables -I INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT
```
- This allowed both the ports for scanning.
