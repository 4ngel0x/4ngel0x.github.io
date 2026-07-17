---
title: CyberDefenders - Gh0stNet Intrusion Lab
date: 2026-07-17 10:00:00 +0100
categories:
  - Writeups
tags:
  - windows
  - sysmon
  - EZ_Tools
description: Writeup · Gh0stNet Intrusion Lab
image:
  path: assets/img/posts/ghostnet-intrusion.png
---
# ESCENARIO
In late May 2026, Kevin Briggs, an analyst at the Chicago investment firm Vireo Financial Group, attended the FinServ Innovation Summit and picked up a branded giveaway at the "DataVault Analytics" booth: a small USB flash drive along with some stickers

One evening at home he plugged it into his personal PC to explore what was on it, he double-clicked what looked like a spreadsheet on the drive. A window flickered and vanished; nothing else seemed to happen, so he thought no more of it. In the background, the disguised launcher had quietly deployed a remote-access trojan that took root on the PC and called out to its operators.

Weeks later, Vireo's security team flagged anomalous activity inside the corporate domain VireoFinancial.local - a valid account behaving strangely, off-hours, from an unfamiliar source. The trail led back to Kevin, and from there to a home PC nobody had been monitoring. You have been brought in to reconstruct the incident across its two stages: the initial compromise on Kevin's home PC, and the hands-on-keyboard intrusion that followed inside the corporate network.

## HERRAMIENTAS


---
## Act 1 - Initial Access
1. A removable USB mass-storage device was the entry point onto the home PC. What friendly name did Windows record for the mounted volume of this device?
- Metemos todas las partes en ftk imager, extareemos el hive SYSTEM y nos vamois a HKLM\SYSTEM\ControlSet001\Enum\SWD\WPDBUSENUM para ver las etiqeutas de los dirves
![](assets/img/posts/Pasted%20image%2020260716203730.png)

2. What is the unique serial number that Windows recorded for that USB mass-storage device?
- Nos dirigimos a HKLM\SYSTEM\ControlSet001\Enum\USBSTOR para ver el serial number del USB.
![](assets/img/posts/Pasted%20image%2020260716204315.png)

3. Provide the exact UTC date and time the USB device was first connected to the host.
- En USBSTOR aparece también este dato
![](assets/img/posts/Pasted%20image%2020260716204652.png)

## Act 1 - Execution
