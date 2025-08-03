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