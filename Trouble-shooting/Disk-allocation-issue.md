# Kali Linux VM disk recovery, boot process recovery and apache/mariadb fix (real incident log)

## 📅 Date
**July 14th, 2025**

## Situation
During a lab session (DVWA vuln related), my kali stopped booting (showed "failed mariaDB start") and the root partition was full.

### What I was able to do successfully:
- edit the boot parameters, to access bash without proper GUI boot.
- Recovered VM without any data loss.
- Extended VM space using Gpartition.
- restored broken system services, like apache2, mariaDB
- learned critical troubleshooting.
- learnt a lot of commands. 

### The reason:
- The **'/dev/sda1'** file was at 100% capacity.
- 'mariaDB' refused to start probably due to disk exhuastion.
- 'Apache2' failed to load due to removal of /var files of apache2, which was caused due to removal of these files while trying to boot by manipulating boot parameters.
- Did change the space but eventually found out that it was just done virtually.

### procedure:
- after accessing bash, mounted the read and write permissions on it using command:
```bash
mount -o remount, rw /
```
- <details> <summary>checked the disk partitioning and got to know what files carry the most amount of size.</summary>
```bash
df -h
du -ah / | sort -rh | head -n 10
```
* **df** is for checking the over all file system, eg. the /dev/sda1 file.
* **du** is for checking in depth, like which file is holding how much space.
* **-h** is for human readable form (eg. in MiBs, GBs).
* **-ah** is for seeing all files in human readable form.
* **-rh** is to sort them in descending order.
* **head -n** is just for printing the first n number of files only.
</details>

- removed some of the files to make sure booting the VM runs properly, successfully ran the services that were failing.
- worked with g partition to actually increase the size (not just virtually).
- fixed the apache /var/log files as they were removed while making space for bootup.
