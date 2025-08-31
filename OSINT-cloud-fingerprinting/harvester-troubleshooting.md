# Tool Installation Troubleshooting

Tried to install `theHarvester` for OSINT but hit APT mirror issues:
```bash
Error: Failed to fetch http://http.kali.org/kali/... 503 Service Unavailable
```
Debugging steps:
- switched to multiple mirrors (kali.org, kali.download, ftp.acc.amu.se)
- Fixed time sync and settled on berkeley mirror.
```bash
echo 'deb http://mirrors.ocf.berkeley.edu/kali kali-rolling main contrib non-free non-free-firmware' | sudo tee /etc/apt/sources.list
```
Installed successfully:
```bash
sudo apt update
sudo apt install theharvester
```

## The Harvester Results:
Attempted:
```bash
theharvester -d 140.245.8.253 -b bing
```
Result:
![harvester Output](/OSINT-cloud-fingerprinting/Screenshots/the-harvester-output.png)
- This was caused as my VM does not have a domain and just has an IP and harvester is for domains.
