# Manual Privilege Escalation & SUID Enumeration
During privilege escalation attempts, I explored SUID binaries and writable directories to identify potential entry points.

## SUID Binary Enumeration
To search for **SUID binaries** (executables with root privileges), I ran this is in the shell bypass i did using `reverse.php`:
```bash
find / -perm -4000 -type f 2>/dev/null
```
#### Explanation:
- `/`: Starts from the root directory.
- `-perm -4000`: Finds files with the SUID permission bit set.
- `-type f`: Restricts results to files (ignores sockets, directories).
- `2>/dev/null`: Suppresses permission-denied errors.
output:
![SUIDs found](/Day-5-notes/Screenshots/SUIDs-found.png)

I attempted brute-forcing common sudo passwords like:
- toor
- John
- Jon123
- www-data

But all attempts failed with authentication errors.

## Writable Directories Enumeration
To find directories writable by the current user (www-data), I ran:
```bash
find / -writable -type d 2>/dev/null
```
This command lists all writable directories.
output:
!![Writable DIRs found](/Day-5-notes/Screenshots/Writable-output-reverse-shell-DVWA.png)
These directories could be potential **injection points**, especially if used by scripts or scheduled tasks.

### Cronjob Files (Privilege Escalation Attempt)
I tried checking common cron directories (/etc/cron.*, /etc/crontab) for writable jobs or custom scripts, but:

- No useful misconfigurations found.
- No scheduled scripts were writable or exploitable.
- No root-owned cronjobs I could hijack.