---
up: "[[Qualcomm Project MoC]]"
related:
  - "[[Rootkits and Bootkits]]"
aliases:
  - OOB Security
created-date: 2024-12-18
status: on-hold
tags:
  - "#type/project"
  - "#qpsi/project"
---

> **Summary**:: OOB Sub-System helps IT Admins to remotely manage and repair office machines. Security of This component is critical since someone can access the machine remotely. 

The main use-cases are remote power on/off, remote wipe, remote diagnostics, etc. This feature is important from a security point of view as it immediately impacts customer and consumer data and execution.

# Project Research

## Fleeting Notes

- [Intel](https://www.intel.com/content/www/us/en/business/enterprise-computers/resources/out-of-band-management.html) and AMD supporting this tech since long.
	- Intel has Intel vPro Enterprise for Windows OS
		- Its based on Intel Active Management Technology (Intel AMT)
		- These RMM hardware-based capabilities operate independently of the OS and provide persistent connectivity
		- With remote keyboard, video, and mouse (KVM) control, IT administrators can access and work on remote PCs and devices as if they were *hands on*.
		- **Intel Mesh Commander** and **MeshCentral** for peer-to-peer remote management.
	- AMD : Out-Of-Band PC Management with AMD PRO Manageability
		- Standards DASH - Desktop & Mobile Architecture for System Hardware
		- DASH is part of DMTF (formerly known as the Distributed Management Task Force).
			- https://www.dmtf.org/standards/dash
		- Couple of Solution
			- EPYC System Management Software (E-SMS)
				- https://www.amd.com/en/developer/e-sms.html
			- Advanced Platform Management Link (APML) Library
				- https://www.amd.com/en/developer/e-sms/apml-library.html
				- https://github.com/amd/esmi_oob_library

- Hypervisor breach may fall in high to medium impact but OOB breach will have sever impact
- We need to prioritize it internally at QPSI. That SS is called OOB SS and it should be our priority but some reason it is not.
- Please work with Nagaraju and Initiate this discussion. We can discuss and make case but any new topic has to go through QPSI vetting process. It means Murali, Nicolas and We should be aligned.

## Qualcomm

- ![[oobmss-qcom-arch.png]]
- https://confluence.qualcomm.com/confluence/display/SecureSystemsGroup/ARB-0286+Out+of+Band+Management+Gen2
- Its a OOBMSS Glymur Compute SoC
- Attack Vector
	- Redfish Protocol SDK
	- MQTT Client
	- OOBMS to TME
	- OOBMS to AP
		- Web Server to HLOS and UEFI.
- Previous efforts
	- Earlier Architectural Review done by Marcels team 2023 [link](https://jira-dc.qualcomm.com/jira/browse/QPSIRAFT-2325)
	- Crypto Library review [link](https://jira-dc.qualcomm.com/jira/browse/QPSIRAFT-2821)
	- Design Review 2024 [link](https://jira-dc.qualcomm.com/jira/browse/QPSIRAFT-2708)

## Product Docs

1. [Glymur Securelib Setup](https://confluence.qualcomm.com/confluence/display/SecureSystemsGroup/Glymur+Securelib+device+setup)
	1. This page has instruction to clone the code and 
	2. From the OOB-TEE service when it receives a packet from the MQTT client via OOB-Main service.
	3. From the QOMS application running on Windows to trigger enrollment
	4.  From the OEM/Enterprise tool running on Windows to pull a QOMS chipset public key
2. [QCOM OOBM Service Specification](https://github.qualcomm.com/pages/CorePlatform/oobm-service/index.html)

## Attack Surface

1.  Some vendor-specific commands may need to be processed by another manager, e.g. an embedded controller or the system OS.
2. Malicious Server
3. Malicious Client
4. Data Format
	1. COSE
	2. QC CBOR
	3. MQTT from zephyr core library.
	4. MBED TLS library crypto data parsing - accepting certificate from untrusted sources.

## Tainted Functions

1. OobSecLib_process
	1. processing incoming request
	2. Verifying the signature on incoming message.
		1. This possibly have vulnerability 
	3. CBOR Data format
2. [QC CBOR implementation](https://corebsp-grok-server.qualcomm.com/source/xref/MPSS.DE.9.0/modem_proc/core/securemsm/services/Qcbor/)
	1. [Buffer Management library](https://corebsp-grok-server.qualcomm.com/source/xref/MPSS.DE.9.0/modem_proc/core/securemsm/services/UsefulBuf/inc/UsefulBuf.h#1023)

## Reference Notes

-  [CBOR RPC 8152 Specs](https://datatracker.ietf.org/doc/html/rfc8152)
	- Concise Binary Object Representation (CBOR) is a data format designed for small code size and small message size
	- CBOR Object Signing and Encryption (COSE)

# Project Management 

## Sub Projects

```dataview
TABLE WITHOUT ID
	file.link as "Sub Projects",
	up, created-date, summary, tags
FROM !"Templates"
WHERE icontains(up, this.file.link)
AND icontains(tags, "type/sub-project")
SORT filename DESC
```

## Meetings

```dataview
TABLE WITHOUT ID
	file.link as "Session",
	up, created-date, participants, summary, tags
FROM !"Templates"
WHERE icontains(related, this.file.link)
AND icontains(tags, "type/meeting")
SORT filename DESC
```

## Tasks and Questions
- 