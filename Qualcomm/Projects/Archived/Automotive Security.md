---
tags:
  - "#qpsi/project"
  - automotive-security
status: abandoned
created-date: 23-11-2022
completed-date: 23-11-2022
---

## Notes

- There are four major components in automotive
	- Car2Could - connecting cars to could
	- Telematic - connect car to cellular network
	- IVI - infotainment system
	- ADAS for cars developed in QNX
- SA515A
- SA2150P
	- you can write application with Qualcomm telematics sdk for QC website
- SDK User Guide [link](https://developer.qualcomm.com/sites/default/files/docs/telematics/user-guide/v1.53.0/)

## Meeting Notes

### 23-11-2022

- Telematics Get Overview - https://docs.qualcomm.com/bundle/80-PE732-70/resource/80-PE732-70.pdf
- Understand Telematics SDK https://developer.qualcomm.com/software/qualcomm-telematics-sdk
- Get list of internal vulnerabilities from orbit and open source vulnerabilities from FOSSID scan reports. Give priority to Critical and High rated vulnerabilities.

## PoC Info

- Liang Cal - he is head of automotive security in QPSI
- Joffrey - fuzzing QNX

## Misc
- Open source vulnerability
	- https://github.com/imp/dnsmasq/search?q=CVE&type=commits
	- https://www.jsof-tech.com/disclosures/dnspooq/
	- https://unit42.paloaltonetworks.com/overview-of-dnsmasq-vulnerabilities-the-dangers-of-dns-cache-poisoning/