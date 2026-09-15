# Week 7, Days 1-2 - SIEM Triage and Incident Response

**Status:** Day 1 completed - Day 2 in progress  
**Environment:** Simulated banking Security Operations Center  
**Focus:** Tier 1 alert triage, runbooks, alert correlation, escalation writing, BEC analysis, containment, and incident-response decision-making

## Overview

This lab focuses on the judgment required in a Security Operations Center. Instead of simply reacting to alert severity, the work requires each alert to be evaluated against user context, network ranges, normal behavior, change records, travel information, and documented runbooks.

Day 1 centers on a twenty-alert queue. Day 2 continues the scenario after Tier 2 confirms that a set of related alerts represents a live business email compromise with a fraudulent wire still at risk.

## Work Completed So Far

### Tier 1 Alert Triage

- Worked through a twenty-alert queue spanning four alert categories:
  - failed logins
  - impossible travel
  - business email compromise indicators
  - suspicious wire transfers
- Applied the appropriate runbook to each alert rather than relying on severity labels alone.
- Recorded a CLOSE or ESCALATE decision with a confidence level and specific justification.
- Checked source addresses against internal and VPN ranges before treating activity as external.
- Used timing, user role, MFA behavior, travel records, and change records as triage context.
- Performed the required distance-and-time reasoning for impossible-travel alerts.
- Used a linked-alerts field to look for relationships across alert categories.

### Escalation Documentation

- Wrote structured escalation notes for every alert that required Tier 2 review.
- Separated observed facts from interpretation.
- Included the applicable runbook criterion.
- Identified the most useful first investigative step for Tier 2.
- Added related-alert references and urgency.
- Added additional reasoning to the least-certain closures describing what evidence would change the decision.

### Day 2 Incident Response - In Progress

The confirmed incident centers on a business email compromise chain involving a lookalike-domain phishing message, suspicious authentication activity, mailbox manipulation, and a fraudulent wire waiting for approval.

Current Day 2 work includes:

- reconstructing the attack chain from the original alerts and investigation evidence
- determining how the attacker obtained access and how MFA was eventually approved
- defining the confirmed scope and remaining unknowns
- prioritizing containment actions around the money still at risk
- verifying that containment actually worked
- documenting the incident timeline and after-action lessons

This page will be updated after the Day 2 response and after-action report are completed.

## Skills and Concepts Applied

- SOC Tier 1 triage
- Runbook-based decision-making
- Alert correlation
- Impossible-travel analysis
- MFA analysis
- BEC indicators
- Wire-fraud triage
- Escalation writing
- Confidence scoring
- Incident scoping
- Containment prioritization
- After-action review

## Three Key Things I Learned

1. **Severity labels are only a starting point.** A low- or medium-severity alert can become important when identity, timing, source network, user role, or related activity changes the context.

2. **Correlation is what turns alerts into an incident.** Authentication, mailbox, phishing, and payment alerts can look separate when they arrive in different queues. Linking them by account, time, and behavior can reveal the full attack chain.

3. **A useful escalation is a decision-support document.** Tier 2 needs the facts, why the activity is suspicious, what runbook criterion applies, which alerts are related, how urgent it is, and the first question to answer next.

## Portfolio Note

This page is an original summary of the work performed and the lessons learned. Proprietary training-manual content, answer keys, and full exercise data are not reproduced.
