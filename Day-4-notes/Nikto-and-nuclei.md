## Nikto
Command Used:
```bash
nikto -h http://127.0.0.1/DVWA/
```
![Nikto basic scan output](/Day-4-notes/Screenshots/Nikto-h-output.png)

### Key Findings:

**Missing Security Headers:**
- X-Content-Type-Options: Can prevent MIME-sniffing attacks.
- X-Frame-Options: Important against clickjacking.

**🕳️ Sensitive Files and Directories:**
- Possible exposure of .git/ directory.
- Found backup and admin-related files.

Potential backdoor indicators (like phpinfo.php).

Nikto scans were noisy — ideal for recon, but may get throttled or slowed by Web Application Firewalls (WAFs).

## Nuclei

### CVEs
command used:
```bash
echo "http://127.0.0.1/DVWA" > target.txt
nuclei -l target.txt -t cves/ -o nuclei_dvwa_scan.txt
```
What this command does:
- -l targets.txt: Feed in a list of URLs to scan.
- -t cves/: Use vulnerability templates from CVEs.
- -o nuclei_dvwa_scan.txt: Save the output results.

Output:
![Nuclei CVEs Output](/Day-4-notes/Screenshots/Nuclei-CVEs-output.png)

Takeaways:
- No CVEs were triggered and it was expected for a controlled training app like DVWA.
- Confirms that the setup works and nuclei is functional.

### misconfigurations
command used:
```bash
nuclei -l target.txt -t misconfigurations/ -o miss_output.txt
```
The output was:
![Nuclei Misconfigurations Output](/Day-4-notes/Screenshots/Nuclei-Misconfigs-output.png)

Templates Loaded: 671
Clusters: 246
Security Issues Detected: 11

| Header                          | Description                                               |
|---------------------------------|-----------------------------------------------------------|
| x-permitted-cross-domain-policies | Prevents Flash/Adobe from accessing restricted resources |
| clear-site-data                 | Clears cookies/cache on logout                           |
| cross-origin-*                 | Protects against cross-origin attacks                     |
| strict-transport-security       | Forces HTTPS-only communication                           |
| permissions-policy              | Controls browser features (camera, geo, etc.)             |
| referrer-policy                 | Controls what referrer data is shared                     |
| x-frame-options                 | Clickjacking protection                                   |
| x-content-type-options          | MIME sniffing prevention                                  |
| content-security-policy         | Defines trusted sources                                   |

- **Nuclei excels when scanning real-world applications. Many template categories (technologies, exposures, etc.) work best outside training environments.**