---
up: "[[Qualcomm Project MoC]]"
related:
  - "[[Rootkits and Bootkits]]"
  - "[[OOB 3rd party analysis]]"
aliases:
  - OOB Security
created-date: 2024-12-18
status: in-progress
tags:
  - "#type/project"
  - "#qpsi/project"
  - type/pinned-docs
  - status/active
summary: OOB Sub-System helps IT Admins to remotely manage and repair office machines. Security of This component is critical since someone can access the machine remotely.
link: https://confluence.qualcomm.com/confluence/display/QPSIFT/Out-of-Band+%28OOBMS%29+fuzzing
---

The main use-cases are remote power on/off, remote wipe, remote diagnostics, etc. This feature is important from a security point of view as it immediately impacts customer and consumer data and execution.

## Goal

The objective of this FR is to improve the security of the for the project to be delivered we have to do the following activity.

1. Code review of the components firmware
2. Fuzzing critical components of the system
	1. For fuzzing we have to fuzz two type of components which is part of external third-party dependencies like MQTT protocol from zephyr
	2. We also have implemented TCP stack which is a abstraction exposed by zephyr firmware.
	3. There are two type of components are dealing with
		1. Third party vendor components like zephyr
		2. Qualcomm's components which are as follows :
			1. File format like QCBOR and ASN1Parser the code for processing this type of data is imported from trustzone which means its already review but it will a good idea to have a fuzzing harness to doing extra level of assurance.
			2. OOB firmware TCP stack - this is vendor dependent layer zephyr doesn't implement this, as it might be hardware dependent it is abstracted.
			3. Since we are accepting TLS certificate, code which is processing this data also have to be fuzzed which is ASN1Parser.
	4. There are also components which are critical in nature because they are processing the data coming from remote sources like TCP, MQTT protocol data. Now securing this component is critical because the data processing is done at pre-authentication level and any vulnerability at this stage can compromise the device remotely without been authenticated which is a very critical issue.
3. Also as part of CRA we have to draw the attack path. This include how compromising one component opens attack to other components.


## PoC

- Eshwar Muppala - mqttclient entrypoints
- Sachin Sharma - ssg designer for ssg oob side (already went through architecture design with Marcel)
- David Collins + Subbaraman Narayanamurthy: core team direct engineers, they will know about QMI pathway from OOB TEE Service to HLOS (they handle OOB TEE Service and OOB NS Service)
- Gerald Dasal - securelib entry point

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
	4. From the OEM/Enterprise tool running on Windows to pull a QOMS chipset public key
2. [QCOM OOBM Service Specification](https://github.qualcomm.com/pages/CorePlatform/oobm-service/index.html)
3. [Zephyr QC Docs](http://go.qualcomm.com/corezephyr)

## Attack Surface

1.  Some vendor-specific commands may need to be processed by another manager, e.g. an embedded controller or the system OS.
2. Malicious Server
3. Malicious Client
4. Data Format
	1. COSE - this also has RFC. This is build on top of CBOR.
	2. QC CBOR - borrowed from TZ code base
	3. MQTT from zephyr core library.
	4. mTLS library crypto data parsing - accepting certificate from untrusted sources.
	5. ASN1 parser - borrowed from TZ code base
5. First the data is deserialzied then the signature verification is done.
6. EC - enterprise chip, request by enterprise. Customer req not sure if it is still active.
7. TCP and MQTT protocol stack on needs to be fuzzed as it is not fuzzed on OSS-Fuzz
8. where is the transport layer stack HAL implement?

## Threat Model
1. Assets - 
	1. RCE in OOB Firmware NS
2. Entry Point
3. External entities
	1. QWES Server
4. External trust levels
5. Major Components
6. Use Scenarios (also useful for CRA)
	1. OOB to UEFI
		1. GLINK to query UEFI
	2. OOB firmware
		1. Can unknown host send mTLS certificate to the firmware(device)?
			1. may be during enrollment
			2. NO ! host is set from HLOS config.
			3. Trigger packet is signed, with license key
			4. locked at the end of enrollment phase.
		2. Can unknown host have contact with firmware network stack?
	3. OOB in Windows
		1. Windows to OOB Entry Point function - OobSecLib_process
		2. OOB to HLOS handling of at least one use-case, SDK/APIs for HLOS clients from OOB Service, IPC interfacing with OOB.
			1. Handled by Core team, send request to HLOS.
			2. There is also back channel for enrollment
			3. Managed by QMI IPC.
	4. Device Enrollment
		1. Initiated by windows interaction only? is the port listening ?
			1. Windows via QMI
	5. Enterprise-specific command that are issued using enterprise key
	6. Per-device certificate which is generated during device enrollment
	7. QMI run on GLINK, and GLINK runs on shared memory. Check only two sub-system are using it. Confused deputy.
	8. How can we attack the storage? change the keys? change host? certificate pinning.
	9. OOB-TEE <--> OOB-NS
		1. Entry Point function - MqttDataProcessor_handleSecLib

---
## CR Raised
> Some of the bugs which you created while working on the project

### Bug Tracker

| ID  | CR                       | Rating | Date       | Comment                                                |
| --- | ------------------------ | ------ | ---------- | ------------------------------------------------------ |
| 1   | https://orbit/CR/4313897 | Medium | 2025-10-08 | OOB write while loading public key from secure storage |
| 2   | https://orbit/CR/4314811 | High   | 2025-10-09 | OOB write while loading public key from MQTT payload   |
| 3   | https://orbit/CR/4314872 | High   | 2025-10-09 | OOB read reading SALT value from MQTT payload          |

---

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
- [Zephyr Fuzzing Docs](https://docs.zephyrproject.org/latest/samples/subsys/debug/fuzz/README.html)

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