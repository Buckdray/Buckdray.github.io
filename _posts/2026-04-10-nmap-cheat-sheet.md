---
title: "Nmap Cheat Sheet — Everything You Need"
date: 2026-04-10 00:00:00 +0000
categories: [Tools]
tags: [nmap, reconnaissance, scanning, cheatsheet]
---

## TL;DR
A practical Nmap reference covering everything from basic discovery 
to aggressive scanning and NSE scripts.

## Basic Scanning

```bash
nmap 10.10.10.1                  # basic scan
nmap -sV 10.10.10.1              # service version detection
nmap -sC 10.10.10.1              # default scripts
nmap -A 10.10.10.1               # aggressive (OS, version, scripts)
nmap -p- 10.10.10.1              # all 65535 ports
nmap -p 22,80,443 10.10.10.1     # specific ports
```

## Speed

```bash
nmap --min-rate 5000 10.10.10.1  # fast scan
nmap -T4 10.10.10.1              # timing template (0-5)
```

## Output

```bash
nmap -oN output.txt 10.10.10.1   # normal output
nmap -oA output 10.10.10.1       # all formats
```

## NSE Scripts

```bash
nmap --script vuln 10.10.10.1         # vuln scan
nmap --script smb-enum-shares 10.10.10.1
nmap --script http-title 10.10.10.1
```

## Takeaways
- Always run `-p-` on HTB/THM — services hide on high ports
- Combine `-sC -sV` as your default starting scan
- `--min-rate 5000` is your best friend on lab machines
