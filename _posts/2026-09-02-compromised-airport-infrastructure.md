---
title: "Compromised Airport Infrastructure"
date: 2026-09-02 00:00:00 +0300
categories: [CyberOps Associate, Network Security]
tags:
  - cyberops
  - wireshark
  - windows
  - blueteam
description: "CyberOps Case 02 write-up covering IDS alerts, VPN and system logs, packet captures, and a compromised airport flight information display environment."
---

<aside class="post-author-note" aria-label="Authorship note">
  <p class="post-author-note-title">NOTE</p>
  <p>The text below has been originally written by the author. ChatGPT GPT-5.6 Sol was used to finalize the wording and presentation of the text.</p>
</aside>

## Case 02

<p class="case-score"><strong>Score:</strong> 24 / 25</p>

<section class="case-evidence-box" markdown="1">
### Evidence Provided

- One `Snort IDS / Sguil` screenshot
- Two `pcap` network captures
- Three text-based log files
</section>

This was the second practical investigation case I completed as part of my CyberOps coursework. The work focused on a suspected compromise in an airport network, where multiple evidence sources had to be reviewed together instead of relying on a single packet capture.

The scenario involved Nangijala International Airport, a smaller international airport with its own IT staff and several systems maintained by external vendors. One of those externally managed systems was the `AirPortSys` flight information display system, which was maintained through secured remote access.

The issue was noticed when the display system began showing unexpected content even though the service still appeared to be running from the outside. The investigation material included `IDS` information, VPN-related data, server logs, and packet captures from the affected environment.

My analysis focused on connecting the alerts and logs with the network traffic to understand how the activity reached the airport infrastructure and what system behavior changed during the incident. The case was a good exercise in building a timeline from several evidence types and separating useful indicators from normal operational noise.

## Incident Briefing

The final submission was prepared as an urgent brieding for the airport management. In the scenario, I was working as a part of the airport's internal IT staff and had been tasked with investigating the incident and determining what had happened.

The briefing presents the key findings of the investigation, the evidence supporting them, the likely impact on the affected systems, and the actions recommended to contain the incident and reduce the risk of further compromise. The original submission was written in Finnish; the English version below is otherwise similar in content.

Thank you for the help on this assignment, KK and AH!

{% include pdf-report.html file="/assets/CyberOps/case02_RikuHakamaki_eng.pdf" title="CyberOps Case 02 investigation report" %}
