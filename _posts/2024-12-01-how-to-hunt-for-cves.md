---
title: "How I Hunt for CVEs — A Practical Guide to Finding Your First Vulnerability"
date: 2024-12-01 00:00:00 +0000
categories: [CVE, Red Team]
tags: [cve, vulnerability-research, bug-hunting, github, osint, php, methodology]
---

## TL;DR
A practical walkthrough of how I find and report CVEs — from picking
targets to writing the disclosure. Includes real GitHub search
queries, dangerous function patterns per language, and tools that
actually work.

## Why Hunt for CVEs?

Beyond the obvious (bug bounty payouts, recognition), CVE hunting
teaches you to read code like an attacker. You develop an instinct
for where developers make mistakes, which makes you better at
everything in security — pentesting, code review, threat modeling.

My first CVEs came from a simple question: *"These open source PHP
projects probably have the same bugs. Let me look."* Six CVEs later,
I had a methodology.

## Phase 1 — Picking the Right Target

The best targets for beginners are:

- **Open source PHP/Python/Node projects** with real users but
  minimal security review
- **Free project templates** (PHPGurukul, SourceCodester,
  CampCodes) — used widely, rarely audited
- **Small open source tools** with active GitHub repos and recent
  commits
- **Self-hosted alternatives** to popular SaaS tools

Avoid: massive projects with active security teams (Apache, Mozilla,
Linux kernel) — those are competitive and require deep expertise.

**The sweet spot:** projects with 50-5000 GitHub stars, real users,
and no security policy or bug bounty page. These are underresearched.

## Phase 2 — GitHub Recon (The Easy Wins)

GitHub code search is one of the most underused tools in vulnerability
research. You can search for dangerous patterns across millions of
repositories.

### PHP — Common Dangerous Functions

```
# Find direct echo of GET/POST params (XSS)
echo $_GET language:PHP

# Find unsanitized SQL queries
mysqli_query($conn, language:PHP

# Find eval usage (code injection risk)
eval($_POST language:PHP

# Find file includes from user input (LFI/RFI)
include($_GET language:PHP
include($_POST language:PHP

# Find dangerous exec calls
exec($_GET language:PHP
system($_POST language:PHP
shell_exec($_REQUEST language:PHP

# Find MD5 password hashing (weak crypto)
md5($_POST['password']) language:PHP
```

### Python — Dangerous Patterns

```
# Command injection
os.system(request. language:Python
subprocess.call(request. language:Python

# Pickle deserialization (RCE)
pickle.loads( language:Python

# SQL injection
cursor.execute(f" language:Python
cursor.execute("SELECT language:Python +format

# SSTI in Jinja2/Flask
render_template_string( language:Python

# Path traversal
open(request. language:Python
```

### JavaScript/Node.js — Dangerous Patterns

```
# Command injection
child_process.exec(req. language:JavaScript

# eval injection
eval(req. language:JavaScript

# NoSQL injection
db.find(req.body) language:JavaScript

# Path traversal
fs.readFile(req.params language:JavaScript

# Prototype pollution
Object.assign({}, req.body) language:JavaScript
```

### Java — Dangerous Patterns

```
# SQL injection
executeQuery(language:Java +"+ request.

# XXE
DocumentBuilderFactory language:Java
SAXParserFactory language:Java

# Deserialization
ObjectInputStream language:Java

# SSRF
new URL(request. language:Java
HttpURLConnection language:Java
```

## Phase 3 — Finding Open Source Tools Online

Beyond GitHub, these sources surface vulnerable targets:

**Project aggregators with source code:**
- [phpgurukul.com](https://phpgurukul.com) — free PHP projects
- [sourcecodester.com](https://www.sourcecodester.com) — PHP/Python projects
- [code-projects.org](https://code-projects.org) — various languages
- [itsourcecode.com](https://itsourcecode.com) — PHP/Python

These sites publish projects specifically for students and developers.
They almost never have security review. They have real users. They
are a goldmine.

**Search for them on Google:**
```
site:sourcecodester.com "free download" php mysql
site:phpgurukul.com download
site:code-projects.org php mysql free
```

**Then check NVD to see what's already been found:**
```
https://nvd.nist.gov/vuln/search/results?query=phpgurukul
```

If CVEs already exist for a project, that's actually a *good* sign
— it means the codebase has vulnerabilities and not all of them have
been found yet.

## Phase 4 — Setting Up Your Lab

Never test against live systems you don't own. Set up locally:

```bash
# Install XAMPP for PHP projects
# Download from apachefriends.org

# Or use Docker
docker run -p 80:80 -v $(pwd):/var/www/html php:8.0-apache

# Install the target app
# Follow their README to set up the database
```

**My testing checklist for every PHP app:**

```bash
# 1. Grep for all user input points
grep -rn "\$_GET\|\$_POST\|\$_REQUEST\|\$_COOKIE" . \
  --include="*.php" | grep -v "htmlspecialchars\|filter_input"

# 2. Find all SQL queries
grep -rn "mysql\|mysqli\|PDO" . --include="*.php" | grep "query\|prepare"

# 3. Find all file operations
grep -rn "file_get_contents\|include\|require\|fopen" . \
  --include="*.php"

# 4. Find all output points
grep -rn "echo\|print\|printf" . --include="*.php"
```

## Phase 5 — Manual Testing

After code review, confirm manually in the browser. Tools to have:

**Burp Suite** — intercept and modify every request. Essential.

**Basic XSS payloads to try:**
```
<script>alert(1)</script>
"><script>alert(1)</script>
'><script>alert(1)</script>
<img src=x onerror=alert(1)>
<svg onload=alert(1)>
javascript:alert(1)
```

**Basic SQLi payloads:**
```
'
''
`
' OR 1=1--
' OR '1'='1
1' ORDER BY 1--
1' UNION SELECT null--
```

**For blind SQLi, use time-based:**
```
1' AND SLEEP(5)--
1'; WAITFOR DELAY '0:0:5'--
```

## Phase 6 — Reporting and Getting the CVE

Once confirmed, report it:

**For PHPGurukul-style projects:**
1. Email the vendor with your findings
2. Give them 30-90 days to respond/patch (responsible disclosure)
3. If no response, proceed to public disclosure

**Request a CVE:**
- Go to [cve.org/ReportRequest](https://www.cve.org/ReportRequest)
- Or report through MITRE directly
- For well-known vendors, they often assign CVEs themselves

**Your report should include:**
```
# Vulnerability Report Template

**Product:** [Name and version]
**Vulnerability Type:** [XSS / SQLi / etc.]
**Affected Component:** [file path and parameter]
**Severity:** [Low/Medium/High/Critical]

**Description:**
[Explain what the vulnerability is]

**Steps to Reproduce:**
1. Navigate to...
2. Enter payload...
3. Observe...

**Impact:**
[What can an attacker do?]

**Proof of Concept:**
[Screenshots or video]

**Remediation:**
[How to fix it]
```

## Common Mistakes to Avoid

**Reporting without PoC** — always include working proof of concept.
Vendors won't take reports seriously without evidence.

**Overestimating severity** — a stored XSS in an admin panel that
requires authentication is Medium, not Critical. Be accurate.

**Testing production systems** — always test locally. You can get
in serious legal trouble testing live systems without permission.

**Not checking if it's already known** — search NVD, GitHub, and
Google before reporting. Duplicate CVEs waste everyone's time.

**Giving up too fast** — if your payload doesn't work, try variants.
WAFs and filters can be bypassed. Developers implement partial
mitigations all the time.

## Tools Summary

| Tool | Use |
|---|---|
| Burp Suite | Intercept, modify, replay requests |
| XAMPP/Docker | Local testing environment |
| grep / ripgrep | Source code pattern search |
| GitHub code search | Find vulnerable patterns at scale |
| NVD / CVE.org | Check known vulnerabilities |
| Nuclei | Automated template-based scanning |
| Nikto | Web server misconfiguration scan |

## Final Thought

CVE hunting is a skill that compounds. Your first one is the hardest
because you don't have the pattern recognition yet. After five or
ten, you develop an eye for it — you can glance at PHP code and spot
the unsafe patterns immediately.

Start with PHPGurukul or SourceCodester. Pick one project, grep for
dangerous functions, set it up locally, and start poking. The bugs
are there. You just have to look.

---

*Check my [vulnerability research repository](https://github.com/Buckdray/vulnerability-research)
for PoCs and write-ups of CVEs I've found.*
