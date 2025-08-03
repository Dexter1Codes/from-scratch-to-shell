# Main Learning 
This session focused learning about analyzing hidden files/directories in web servers using tools like `curl, Dirb, Gobuster` on DVWA.

### robots.txt Enumeration
I began by accessing `robots.txt` file:
```bash
curl http://127.0.0.1/DVWA/robots.txt
```
output was:
```bash
Output:
User-agent: *
Disallow: /
```
This tells us that all user-agents are not allowed to access any part of the site. While this is a directive for search engines (not a security measure), it often hints that the site is under development. Hackers or analysts might interpret it as a clue to investigate deeper. Best case for us that we can work on it.

### Note: robots.txt doesn't provide any kind of protection or security, it is just a polite request.

## Directory Brute Forcing with Dirb
Dirb is a command-line tool that does Directory-based brute-forcing on web server or websites and discover hidden web content (files and directories).

**BASIC scan**
```bash
Dirb HTTP://127.0.0.1/DVWA/
```
output:
![Dirb Basic Output](/Day-4-notes/Screenshots/Dirb-basic-output.png)
- This showed us that there are directories and files:
    1. some of them are directly accessible (code 200).
    2. some of them are moved temporarily (code 302).
    3. some of them are moved permanently (code 301).
    
to customize file extensions during scan:
```bash
Dirb http://127.0.0.1/DVWA/ /usr/share/wordlists/dirb/common.txt -X .php, .php.old, .txt, .bak, .php.bak
```
Output was:
![Dirb Extension-output](/Day-4-notes/Screenshots/Dirb-extension-output.png)

- This instructs Dirb to append all these at the end of each word in the wordlist.

**Note:** Dirb is effective but slow, and binary outputs from `curl -i` may just clutter the terminal as they're not readable.

### Backup & Residual File Discovery
Websites sometimes leave behind outdated or sensitive files like configuration backups.
We tried manual probing using:
```bash
curl -I http://127.0.0.1/DVWA/config.php.bak
curl -I http://127.0.0.1/DVWA/config.php.old
curl -I http://127.0.0.1/DVWA/config.php~
curl -I http://127.0.0.1/DVWA/config.bak
```
output:
![curl manual bruteforcing](/Day-4-notes/Screenshots/curl-manual-bruteforce.png)
- In our case, none of these files existed, but on real-world targets, residual files can provide a way to sensitive data.

## GoBuster
GoBuster offers all these over dirb:
1. **multiprocessing** -> faster scans
2. **various Extensions** -> like basic directory scan, DNS scan, VHost scan
3. customizable output and Verbosity (Detail of the output the tool gives)

Command used:
```bash
gobuster dir -u http://127.0.0.1/DVWA/ -w /usr/share/wordlists/dirbuster/raft-medium-directories.txt
```
output:
![GoBuster medium raft output](/Day-4-notes/Screenshots/GoBuster-medium-raft-ouptut.png)
by this what we did was:
- verify that the file exists (Accessed it using Github)
- The output revealed Directories like:
    - /config/
    - /docs/
    - /tests/
    - /database/
    - /external/

- The error I saw was not the command failing, but its just a ASCII related issue from the file `x1flog` (an edge case).