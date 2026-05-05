---
title: "HTB Kigali Meetup #5 — Threat Hunting on Steroids"
date: 2026-02-14 10:00:00 +0200
categories: [Blue Team, Meetup]
tags: [threat-hunting, siem, detection, htb-kigali]
---

## TL;DR
Meetup #5 focused on what SIEM alerts miss and how to hunt threats
proactively. Here are my notes and key takeaways from the session.

## What We Covered
Threat hunting goes beyond waiting for alerts to fire. The session
walked through how attackers operate below the detection threshold —
living off the land, abusing legitimate tools, and blending into
normal traffic.

## Key Concepts

**Why alerts aren't enough**
- Alerts are reactive — they fire after a signature is matched
- Skilled attackers deliberately avoid triggering known signatures
- Threat hunting is proactive — you go looking instead of waiting

**The hunting process**
1. Form a hypothesis — *"What if an attacker is using PowerShell to move laterally?"*
2. Define what evidence would confirm or deny it
3. Query your logs/SIEM for that evidence
4. Iterate based on findings

**Tools discussed**
- Splunk / Elastic for log querying
- Sigma rules for portable detection logic
- MITRE ATT&CK as a framework for hypothesis generation

## My Takeaways
- Start hunts with ATT&CK techniques, not random log diving
- Document every hunt — negative results are still valuable
- Correlate across multiple data sources, not just endpoint logs

## Resources
- [MITRE ATT&CK](https://attack.mitre.org)
- [Sigma Rules](https://github.com/SigmaHQ/sigma)
- [HTB Kigali Discord](https://discord.gg/ktUNhTVNBM)
