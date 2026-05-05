---
title: "Blue Team Basics: Understanding SIEM Alerts"
date: 2026-04-08 00:00:00 +0000
categories: [Blue Team]
tags: [siem, splunk, detection, defensive, logging]
---

## TL;DR
A SIEM is only as good as the rules feeding it. This post breaks down 
how alerts work, what makes a good detection rule, and what to look 
for as a SOC analyst.

## What is a SIEM
A Security Information and Event Management (SIEM) system collects 
logs from across your environment — firewalls, endpoints, servers — 
and correlates them to surface suspicious activity.

Popular SIEMs:
- **Splunk** — industry standard, massive ecosystem
- **Elastic SIEM** — open source friendly
- **Microsoft Sentinel** — Azure native

## Anatomy of a Good Alert
A well-written detection rule has three things:

1. **A specific trigger** — not "any failed login" but "5 failed logins 
in 60 seconds from the same IP"
2. **Context** — what asset, what user, what time
3. **A low false positive rate** — noisy alerts get ignored

## Common Detections to Know
| Alert                        | What it means        |
| ---------------------------- | -------------------- |
| Multiple failed logins       | Brute force attempt  |
| Login outside business hours | Potential compromise |
| Large outbound data transfer | Data exfiltration    |
| New admin account created    | Privilege escalation |
| PowerShell encoded command   | Malware execution    |

## Takeaways
- Tune your rules — raw default rules will drown you in noise
- Always correlate alerts with other events before escalating
- Understand the attacker's perspective to write better detections
