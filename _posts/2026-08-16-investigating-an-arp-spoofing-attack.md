---
title: "Investigating an ARP Spoofing Attack"
date: 2026-08-16 00:00:00 +0300
categories: [CyberOps Associate, Network Security]
tags:
  - cyberops
  - wireshark
  - reporting
  - blueteam
description: "CyberOps Case 01 write-up covering ARP spoofing, IP impersonation, and a Man-in-the-Middle attack found in packet evidence."
---

<aside class="post-author-note" aria-label="Authorship note">
  <p class="post-author-note-title">NOTE</p>
  <p>The text below has been originally written by the author. ChatGPT GPT-5.6 Sol was used to finalize the wording and presentation of the text.</p>
</aside>

## Case 01

<p class="case-score"><strong>Score:</strong> 19 / 20</p>

<section class="case-evidence-box" markdown="1">
### Evidence Provided

- One `pcap-file` network capture
</section>

This was the first practical investigation case I completed as part of my CyberOps coursework. A network traffic capture was provided for analysis, and I used `Wireshark` to determine what had happened and document the findings clearly.

The scenario involved HaiTek Company Ltd, where suspicious activity suggested that sensitive archive-server traffic may have been exposed. The goal was to examine the supplied `PCAP`, identify the relevant hosts, and explain whether the packet evidence supported a real incident.

The investigation focused on IP and MAC address relationships in the capture. The key anomaly was that Peter Sunshine's IP address, `172.17.0.40`, appeared in the ARP table with two different MAC addresses at the same time. From there, I followed the traffic involving the archive server and correlated the suspicious activity with a Raspberry Pi device.

The evidence indicated an `ARP spoofing` attack. The Raspberry Pi impersonated Peter's network identity, positioned itself as a `Man-in-the-Middle`, and was able to see the archive-server traffic related to the offer file Peter was viewing. After that activity ended, the Raspberry Pi left the network and Peter's correct MAC address returned to the ARP table.

## Investigation Response

The final submission was written as a response to the fictional client who had commissioned the investigation. In the scenario, I had been hired by the affected company to investigate the suspected security incident and report back with my findings.

The response summarizes the investigation results, explains what happened based on the packet-analysis evidence, and includes the relevant technical evidence supporting the conclusion. The original submission was written in Finnish; the English version below is otherwise similar in content.

Thank you for the help on this assignment, KK!

{% include pdf-report.html file="/assets/CyberOps/case01_RikuHakamaki_eng.pdf" title="CyberOps Case 01 investigation report" %}
