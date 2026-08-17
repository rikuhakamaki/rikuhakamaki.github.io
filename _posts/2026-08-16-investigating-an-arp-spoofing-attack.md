---
title: "Investigating an ARP Spoofing Attack"
date: 2026-08-16 00:00:00 +0300
categories: [CyberOps, Network Security]
tags:
  - cyberops
  - wireshark
  - reporting
  - blueteam
description: "CyberOps Case 01 write-up covering ARP spoofing, IP impersonation, and a Man-in-the-Middle attack found in packet evidence."
---

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

## Full Investigation Report

The full investigation report, including the supporting packet-analysis evidence and final conclusion, can be viewed below. The report was originally written in Finnish; this English version is otherwise similar in content.

{% assign case01_report = '/assets/CyberOps/case01_RikuHakamaki_eng.pdf' | relative_url %}

<div class="pdf-report">
  <iframe src="{{ case01_report }}" title="CyberOps Case 01 investigation report" loading="lazy"></iframe>
</div>

<p class="pdf-report-link">
  <a href="{{ case01_report }}" target="_blank" rel="noopener">View PDF directly</a>
</p>
