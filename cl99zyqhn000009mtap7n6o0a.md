---
title: "picoCTF 2022: Binary Exploitation: CVE-XXXX-XXXX"
seoDescription: "Enter the CVE of the vulnerability as the flag with the correct flag format: picoCTF{CVE-XXXX-XXXXX} replacing XXXX-XXXXX with the numbers for the match..."
datePublished: Sat Oct 15 2022 14:11:29 GMT+0000 (Coordinated Universal Time)
cuid: cl99zyqhn000009mtap7n6o0a
slug: picoctf-2022-binary-exploitation-cve-xxxx-xxxx
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1665843077766/bDp6jThex.png
tags: cyber-security, ctf, binary-exploitation, picoctf, picoctf-2022

---

## Introduction
**Challenge:** [CVE-XXXX-XXXX](https://play.picoctf.org/practice/challenge/262?originalEvent=70&page=1)

**Category:** Binary Exploitation

**Description:**

Enter the CVE of the vulnerability as the flag with the correct flag format:
`picoCTF{CVE-XXXX-XXXXX}` replacing `XXXX-XXXXX` with the numbers for the matching vulnerability.
The CVE we're looking for is the first recorded remote code execution (RCE) vulnerability in 2021 in the Windows Print Spooler Service, which is available across desktop and server versions of Windows operating systems. The service is used to manage printers and print servers.

## Solution
This one is actually a pretty simple and straightforward challenge. We need to find the CVE of the first recorded remote code execution (RCE) vulnerability in 2021 in the Windows Print Spooler Service. For this, we can just Google it. I Googled "first recorded remote code execution (RCE) vulnerability in 2021 in the Windows Print Spooler Service" and got my result, which is `CVE-2021-34527`. So we got our flag which is `picoCTF{CVE-2021-34527}`. Okay, so, now let's explore more.

What is CVE? CVE, short for Common Vulnerabilities and Exposures, is a list of publicly disclosed computer security flaws. 

What is Remote Code Execution? Remote code execution (RCE) attacks allow an attacker to remotely execute malicious code on a computer. The impact of an RCE vulnerability can range from malware execution to an attacker gaining complete control over a compromised machine.

What is the PrintNightmare (CVE-2021-34527)? PrintNightmare (CVE-2021-34527) is a vulnerability that allows an attacker with a regular user account to take over a server running the Windows Print Spooler service. This service runs on all Windows servers and clients by default, including domain controllers, in an Active Directory environment.

## Conclusion
Basically, In this article, we learned about CVE, RCE, and PrintNightmare Vulnerability. And we have found our flag by using the CVE number.

**Flag:** `picoCTF{CVE-2021-34527}`