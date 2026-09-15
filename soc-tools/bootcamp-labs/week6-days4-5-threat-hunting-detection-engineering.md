# Week 6, Days 4-5 - Threat Hunting and Detection Engineering

**Status:** Completed  
**Environment:** Simulated banking environment  
**Focus:** Threat hunting, scoping, MITRE ATT&CK, Pyramid of Pain, Sigma detections, containment, eradication, recovery, and post-incident review

## Overview

This lab continued directly from the prior forensic investigation. The objective shifted from explaining one compromised host to answering broader incident-response questions: how long the intrusion had actually been active, whether the attacker moved to other systems, what data was at risk, and how to turn forensic findings into repeatable detections.

## What I Did

### Extended the Intrusion Timeline

- Used perimeter network telemetry retained longer than host logs to look beyond the original disk-based timeline.
- Identified attacker activity that predated the evidence visible on the imaged host.
- Compared large outbound transfers and recurring connection patterns to determine which activity required deeper investigation.
- Merged network and host evidence into one extended incident timeline.

### Hunted Across the Fleet

- Built a host-by-indicator hunting matrix across the server fleet.
- Reviewed authentication logs for signs of lateral movement.
- Confirmed lateral movement from the initially compromised web server to a second server through an attacker-controlled service account and harvested SSH key.
- Compared process and network telemetry across systems.
- Documented which systems were compromised and which were assessed as clean, including exactly what was checked on the clean hosts.
- Distinguished a legitimate five-minute monitoring-agent check-in from malicious beaconing by validating the process, destination, schedule, and asset record.

### Reworked Indicators Using the Pyramid of Pain

- Expanded the original IOC list with indicators discovered during fleet hunting.
- Classified indicators by Pyramid of Pain tier.
- Compared low-cost indicators such as IP addresses and hashes against higher-value behavioral indicators.
- Reframed the intrusion as attacker behaviors that could be detected even if filenames, addresses, or hashes changed.

### MITRE ATT&CK and Sigma Detection Engineering

- Mapped the intrusion chain to MITRE ATT&CK tactics and techniques.
- Connected each ATT&CK mapping to supporting evidence.
- Wrote four Sigma detection concepts focused on:
  - suspicious persistence executing binaries from non-standard locations
  - script files written into web upload directories
  - encoded directory traversal against CGI paths
  - service-account SSH logins originating from server-segment hosts
- Considered realistic false positives and appropriate severity levels for each detection.

### Containment, Eradication, Recovery, and Review

- Built an ordered containment, eradication, and recovery plan.
- Considered evidence preservation before destructive remediation.
- Identified the need to restore from a backup generation that predates compromise.
- Connected recovery verification to the original indicators and behaviors.
- Completed a post-incident review focused on the detection gap and improvements that could have surfaced the intrusion earlier.

## Skills and Concepts Applied

- Threat hunting
- Hypothesis-driven investigation
- Network telemetry analysis
- Lateral-movement analysis
- SSH authentication review
- Fleet scoping
- Pyramid of Pain
- MITRE ATT&CK
- Sigma
- Detection engineering
- Containment planning
- Eradication and recovery
- Post-incident review

## Three Key Things I Learned

1. **Behavior is harder for an attacker to change than an IOC.** IP addresses, hashes, and filenames can be replaced quickly. Detecting a root-owned process established through persistence and maintaining suspicious outbound communication forces the attacker to change technique, not just configuration.

2. **A threat hunt has to prove its scope.** Finding a second compromised host is only half the job. A defensible scoping report must also state what was checked on systems assessed as clean so leadership understands the limits of the conclusion.

3. **Detection engineering should come directly from incident evidence.** The most useful detections were not one-time matches for a known address. They translated attacker behavior from the investigation into reusable rules that could identify similar activity in the future.

## Portfolio Note

This page is an original summary of the work performed and the lessons learned. Proprietary training-manual content, answer keys, and full exercise data are not reproduced.
