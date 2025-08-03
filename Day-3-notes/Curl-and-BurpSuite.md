# Curl and BurpSuite Exploitation

## Curl
This is for a command-line tool for making HTTP requests. This is a go-to for security testing, especially in scenarios where you want to manually inspect headers, redirects and simulate browser behavior without GUI.

### Basic Commands and Observations
```bash
curl http://127.0.0.1/DVWA/
curl -i http://127.0.0.1/DVWA/
curl -iL http://127.0.0.1/DVWA/
```
- The first one was a basic request and didn't give any output as it was redirected to the Login Page (which will be observed further).
- the second command displays HTTP headers along with response code:
![Curl -i Output](/Day-3-notes/Screenshots/Curl-i-output.png)
    1. **302-Found**: Indicates redirection to another page.
    2. **Set-cookie**: This tells us the ***PHPSESSIONID***, ***security Levels***, etc.
        - The **SameSite=Strict** tells us about the way this handles cookies from different websites by saying *Strict*, it means that there won't be any cookie sent if request comes from different domain.
    3. **Cache-Control:No-store**: Ensures no sensitive data is stored in cache.

- The last command was to follow that redirect, which we got to know was Login Page of the website.

**Even after DVWA Security Level Set to Impossible, most of the MetaData is extractable using Curl**

### Bypassing with Cookie Injection
DVWA higher security Levels prevent anonymous Interaction. To try cookie injection i took the PHP session ID from the Storage tab by visiting the website on the browser and exploring the DevTools.
```bash
curl -A "Mozilla/5.0" \ -H "Cookie: PHPSESSID='the session id we extracted'" \ HTTP://127.0.0.1
```
The output was:
![Curl Cookie Injection](/Day-3-notes/Screenshots/Curl-Cookie-Injection.png)

### Saving Cookies To Files
```bash
curl -c cookies.txt HTTP://127.0.0.1/DVWA/login.php
```
-This command stores the following data (which is usually shown as cookie):
![Curl Cookie File](/Day-3-notes/Screenshots/Extracted-cookies.png)


## Burp Suite
The basic flow of working was:
Setup BurpSuite -> Enable Proxy -> Intercept login Req -> Analyze/modify in Repeater
**Proxy Req:**
![BurpSuite Proxy HTTP Req](/Day-3-notes/Screenshots/BurpSuite-Proxy-HTTP.png)

### Observations 
- Manual SQL payloads like `admin' OR '1'='1` failed to bypass the login in Impossible mode.
- After several manual attempts, crafted a GET request that worked like a charm:
```bash
GET /DVWA/vulnerabilities/brute/?username=admin'OR'1'='1&password=password&Login=Login HTTP/1.1
```
- The request was getting redirected to Login page continuously, so we can say that:
    1. inpurt sanitization is strong.
    2. CSRF tokens are not being bypassed properly.
    3. or session/cookie validation is failing.

### 🚧 DVWA’s *Impossible* security mode is clearly working as intended, rejecting even well-known SQLi bypasses.
