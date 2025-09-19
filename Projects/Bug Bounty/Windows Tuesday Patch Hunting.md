---
tags:
  - "#vulnerability-research"
  - "#windows-patch"
  - "#type/project/personal"
up: "[[Personal Project MoC]]"
related: 
aliases: 
created-date: 2024-12-21
status: todo
---

> **Summary**:: The Goal of this project is the understand how to analyse windows security patches and creating exploit for the patches.

## Exploit Targets

| CVE | Area | Bug Type | CVSS Score | Comment |
|---|---|---|---|---|
| CVE-2023-24871 | Bluetooth | RCE | 7.7 | Exploitation Less Likely |
| CVE-2023-23392 |  HTTP driver HTTP/3 protocol| RCE | 8.5 | [[Patch Diffing Windows HTTP Vulnerability]]
| CVE-2023-23410 |  HTTP driver | EoP | 6.8 | [[Patch Diffing Windows HTTP Vulnerability]] |
| CVE-2023-21554 | MSMQ - QueueJumper | | [Unauthorized Remote Code Execution](https://blog.checkpoint.com/security/watch-out-critical-unauthorized-rce-vulnerability-in-msmq-service/) |
| CVE-2023-28231 | DHCP Server Service | 7.7 | An unauthenticated attacker could leverage a specially crafted call to the DHCP service to exploit this vulnerability. |


- Bluetooth  
	- Diffing bthport.sys leads to BthLeLib_ADValidateEx() getting patched to include a early call to perform some basic validation of an Bluetooth low energy Advertisment via BthLeLib_ADValidateBasic()  
	- Eventually following X’refs leads to BthLE_ProcessAdvertisementEvent()  
  
Not sure if it’s anything but maybe others know more about windows BT Low Energy Advertisements?

## Study Material

- [Pwning Windows Ancillary Function Driver for WinSock (afd.sys) in 24 Hours](https://securityintelligence.com/posts/patch-tuesday-exploit-wednesday-pwning-windows-ancillary-function-driver-winsock/)
- [CVE North Stars Research Group](https://cve-north-stars.github.io)

## Tasks and Questions

- [ ]

---
## Notes
- 

---

## References
- 