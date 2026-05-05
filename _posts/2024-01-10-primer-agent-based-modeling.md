---
title: "Back in the Day: A Primer to Agent-Based Modeling"
date: 2024-01-10 00:00:00 +0000
categories: [Journey, Research]
tags: [agent-based-modeling, netlogo, research, masters, simulation, bug-bounty]
---

## TL;DR
During my masters I used Agent-Based Modeling (ABM) to study the 
dynamics of bug bounty program design. This is my attempt to explain 
ABM to a security audience — what it is, why it matters, and how I 
used it to simulate hacker behavior at scale.

## A Different Kind of Thinking

Most of us in security think in terms of systems — firewalls, 
exploits, attack chains. But during my masters I had to think 
differently. My research question wasn't *"how do you exploit this 
system"* — it was *"how do you design a bug bounty program that 
actually works?"*

That question led me down a rabbit hole into Agent-Based Modeling, 
a simulation methodology that lets you model complex systems by 
simulating the individual actors within them — and watching what 
emerges.

## What is Agent-Based Modeling?

Traditional modeling asks: *what equation describes this system?*  
ABM asks: *if each individual follows these rules, what does the 
system do?*

The difference sounds subtle but it's profound. Consider a bug 
bounty program. You could model it with equations — payout rate, 
number of researchers, vulnerability severity distribution. But that 
misses the human element entirely. Real researchers:

- Have different skill levels
- Choose programs based on reputation and payout
- Learn from each other
- Give up if rewards feel unfair
- Compete for the same bugs

None of that is captured in a formula. ABM captures all of it.

## The Three Core Components

Every ABM has the same building blocks:

**1. Agents**  
The individual actors in your model. In my research these were:
- **Researchers** — hunters with varying skill levels, motivations, 
and bandwidth
- **Vendors** — companies running programs with different budgets, 
scope definitions, and responsiveness
- **Vulnerabilities** — bugs of varying severity sitting in a 
simulated attack surface

**2. Environment**  
The space agents live and interact in. This could be a physical 
grid, a network topology, or an abstract market — whatever makes 
sense for your system.

**3. Rules**  
Simple behavioral rules each agent follows. The magic of ABM is 
that complex system behavior *emerges* from simple individual rules. 
No one programs the emergent pattern — it just appears.

## Why NetLogo?

NetLogo is the go-to tool for ABM research. It was developed at 
Northwestern University and has a massive library of example models 
spanning everything from disease spread to economics.

For security research it's particularly useful because:

- The visual interface lets you *watch* your simulation run in 
real time
- You can adjust parameters mid-run and see effects immediately
- Built-in statistical tools make analysis straightforward
- The syntax is simple enough that domain experts (not just 
programmers) can build models

A basic NetLogo agent definition looks like this:

```netlogo
turtles-own [
  skill-level      ; researcher capability 0-100
  motivation       ; willingness to hunt 0-100
  earnings         ; total rewards collected
]

to hunt
  if motivation > 50 [
    let target find-vulnerability
    if skill-level > [difficulty] of target [
      report-bug target
    ]
  ]
end
```

Each researcher (turtle) has properties and follows simple rules. 
Run thousands of them simultaneously and patterns emerge.

## Applying ABM to Bug Bounty Design

Bug bounty programs are a coordination problem. You have:

- A vendor who wants vulnerabilities found cheaply
- Researchers who want fair compensation for their time
- A competitive market where both sides have asymmetric information

My research used ABM to explore questions like:

**What payout structure attracts the best researchers?**  
Flat fees vs. severity-tiered payouts vs. percentage-of-impact 
pricing produce very different researcher communities in simulation.

**How does scope definition affect coverage?**  
Narrow scope concentrates effort but leaves blind spots. Broad scope 
spreads effort thin. ABM lets you find the threshold.

**What happens when researchers share information?**  
When agents were allowed to share vulnerability patterns, overall 
program effectiveness increased — but duplicate submissions also 
spiked. This mirrors real platforms like HackerOne and Bugcrowd 
where disclosure norms are actively debated.

**How does response time affect researcher retention?**  
In simulation, slow triage responses above a certain threshold 
caused a cascade — top researchers left, attracting lower-skill 
hunters, which further degraded program quality. A death spiral 
that matches anecdotal evidence from real programs.

## What ABM Taught Me About Security

Working on this research fundamentally changed how I think about 
security programs. A few things that stuck:

**Emergent behavior is real.** You cannot predict what a bug bounty 
ecosystem will do by looking at individual incentives alone. The 
interactions between researchers, vendors, and platform rules create 
behaviors no one designed.

**Small rule changes have large effects.** Changing a single 
parameter — say, response time SLA — could collapse researcher 
participation in simulation. Real programs have learned this the 
hard way.

**Diversity matters.** Programs that attracted only elite 
researchers found fewer total bugs than programs that maintained a 
diverse researcher pool. Beginner hunters find beginner bugs that 
experts skip over.

**Security is a complex adaptive system.** Attackers, defenders, 
platforms, researchers — they all adapt to each other. Any model 
that treats this as static is wrong.

## Getting Started with ABM

If this sounds interesting and you want to explore it:

1. **Download NetLogo** — free at [ccl.northwestern.edu/netlogo](https://ccl.northwestern.edu/netlogo/)
2. **Explore the models library** — hundreds of pre-built models 
ship with the software
3. **Start with Wolf Sheep Predation** — the classic intro model 
that demonstrates emergence beautifully
4. **Read the textbook** — *Introduction to Agent-Based Modeling* 
by Wilensky & Rand (MIT Press) is the definitive reference

For security folks specifically, think about what systems in your 
domain have lots of interacting agents making decisions. Phishing 
campaigns, malware propagation, patch adoption, insider threat — 
all of these have been studied with ABM and the literature is rich.

## Closing Thought

My masters research lived at the intersection of cybersecurity, 
economics, and complexity science. It wasn't the most straightforward 
path but it gave me a mental model that I use constantly — the idea 
that you can't understand a security ecosystem by studying its parts 
in isolation. The interactions are where the interesting stuff lives.

If you're doing any kind of security program design — bug bounty, 
red team, security awareness — think about the agents, their rules, 
and what might emerge. You might be surprised what your intuitions miss.

---

*This post is based on research published during my masters. Full 
paper available on [IEEE Xplore](https://ieeexplore.ieee.org/document/10430013/).*
