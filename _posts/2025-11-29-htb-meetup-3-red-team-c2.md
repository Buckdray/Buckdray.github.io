---
title: "HTB Kigali Meetup #3 — Red Team C2 Foundations"
date: 2025-11-29 10:00:00 +0200
categories: [Red Team, Meetup]
tags: [c2, command-and-control, red-team, cobalt-strike, htb-kigali]
---

## TL;DR
Meetup #3 broke down Command and Control (C2) infrastructure —
how red teamers set it up, how it works, and how defenders can spot it.

## What is C2?
A Command and Control framework lets an attacker maintain persistent
access to a compromised machine and issue commands remotely.

```
Attacker Machine → C2 Server → Beacon/Agent → Compromised Host
```

## C2 Frameworks Discussed

| Framework | Type | Notes |
|---|---|---|
| Metasploit | Open source | Industry standard, noisy |
| Cobalt Strike | Commercial | Most used by APTs |
| Sliver | Open source | Modern C2, actively developed |
| Havoc | Open source | Cobalt Strike alternative |

## Key Concepts

**Beacons** — lightweight agents that check in to the C2 server
on a schedule (sleep timer) to receive commands and send results.

**Malleable profiles** — customize how beacon traffic looks to
evade network detection. Make C2 traffic look like normal HTTPS.

**Redirectors** — proxy servers between the victim and the actual
C2 server to protect infrastructure from being burned.

## Detection Opportunities
- Unusual outbound connections on standard ports (80, 443)
- Regular beaconing intervals in network logs
- Encoded PowerShell execution
- Suspicious parent-child process relationships

## My Takeaways
- Understanding C2 from the offensive side makes you a better defender
- Sleep jitter and malleable profiles make C2 detection much harder
- Focus detection on process behavior, not just network traffic

## Resources
- [Sliver C2](https://github.com/BishopFox/sliver)
- [HTB Kigali Discord](https://discord.gg/ktUNhTVNBM)
