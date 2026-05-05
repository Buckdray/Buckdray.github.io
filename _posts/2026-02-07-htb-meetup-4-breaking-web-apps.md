---
title: "HTB Kigali Meetup #4 — Breaking Modern Web Apps"
date: 2026-02-07 10:00:00 +0200
categories: [Red Team, Meetup]
tags: [web-security, recon, exploit, burpsuite, htb-kigali]
---

## TL;DR
Meetup #4 walked through the full lifecycle of web app hacking —
from recon to exploit. Notes from the session below.

## Recon Phase
Before touching a web app, understand its surface:

```bash
# subdomain enumeration
subfinder -d target.com
ffuf -w wordlist.txt -u https://target.com/FUZZ

# technology fingerprinting
whatweb https://target.com
wappalyzer
```

## Common Vulnerabilities Covered

**IDOR (Insecure Direct Object Reference)**
- Change `/api/user/1234` to `/api/user/1235`
- If you see another user's data — that's an IDOR

**SQL Injection**
```sql
' OR 1=1--
' UNION SELECT null,username,password FROM users--
```

**XSS (Cross-Site Scripting)**
```javascript
<script>alert(document.cookie)</script>
"><img src=x onerror=alert(1)>
```

## Tools Used
- Burp Suite — intercept and modify requests
- ffuf — directory and parameter fuzzing
- SQLMap — automated SQL injection testing

## My Takeaways
- Always map the full attack surface before exploiting anything
- Chain small bugs together — one IDOR + one XSS = account takeover
- Document everything with screenshots for writeups

## Resources
- [PortSwigger Web Academy](https://portswigger.net/web-security)
- [HTB Kigali Discord](https://discord.gg/ktUNhTVNBM)
