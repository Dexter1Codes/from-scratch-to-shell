## 🔢 Nikto

```bash
nikto -h http://127.0.0.1/DVWA
```

Nikto checks for:
- Misconfigured headers
- Default/admin files
- Backup files
- Dangerous dirs

- Findings:
    * found that there are possible backdoors
    * Found potential .git directories
    * Saw missing headers like X-Content-Type-Options (for preventing sniffing) and X-Frame-Options (for preventing clickjacking)

- Detected some dirs could be dangerous if not secured

Also tried scanning a public site. It froze partway — probably due to WAFs (Web App Firewalls) slowing or blocking Nikto's probing. They don't outright block it, but slow it down to the point it's unusable.