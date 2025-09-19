---
up: "[[Qualcomm MoC]]"
tags:
  - "#qpsi/report"
status: done
created-date: 2021-12-13
summary: Goal planning for 2021
---

# Projects

## 2020-21

1. [[QDI Driver Fuzzing]]
2. [[eAI Driver Fuzzing]]
3. [[EVS vocodec Fuzzing]]
4. [[Codec Fuzzing]] - OMX Integration (Android Multimedia)
5. [[WIN 7 - Vulnerability Detection]]
6. Auto HLOS Driver fuzzing
7. [[Lassen]] - Modem on x86


eAI Driver Fuzzing
Understand the vulnerability Detection needs for eAI Machine Learning Driver and deploy effective process for early vulnerability detection activities.

Update:
Working on the new version of the fuzzer to test in new version of the code base.

WiFi 7 - Vulnerability Detection
Understand the Vulnerability Detection state of affairs for WIN team test set up and suggest possible improvements/efficiencies

Update : 
Working with the team to get the hardware setup ready so that we can start with setting up syskaller for fuzzing. 

EVS vocodec Fuzzing
Fuzzing EVS Voice-Codec code base before and after REL 16 upgrade

Update:
3 vulnerabilities have been identified and corresponding CR's are raised. Tech team has fixed issues.

Android Multimedia Codec Fuzzing
This project was an effort to fuzzing audio codec which were not fuzzed before by the team. During the research on existing fuzzer I came across older research "Fuzzing Android OMX " which reported several bugs to Various vendor including Qualcomm. The bugs which were reported earlier are still present in our code base in which same pattern.
Update: I am working with the tech team to got those bugs fixed.   

### Status Update
## 2024

- FastRPC Code Review - 3 CR's
- Automotive HGSL - 2 CR's
- 

### Submitted Yearly Goals

1. Linux Driver to do Research on Qualcomm's Devices
	- Custom Linux Divers to execute kernel shellcode
2. Fuzzing Windows Kernel Driver
	1. Fuzzing Qualcomm Kernel Divers in Windows for example FastRPC, Graphics Driver, etc
3. Hypervisor Fuzzing
	1. Mapping out attack surface for Gunyah Hypervisor and Fuzzer the critical components of the Hypervisor Stack
4. Linux Kernel Fuzzing with Syzkaller
	1. Fuzzing Different Qualcomm Driver for Linux Android Kernel and improve the coverage
5. Understand Qemu
	1. Improve understanding of Qemu emulator so that we can load different Qualcomm Firmware and fuzz it.

### Action Projects

- [[Camera Driver Fuzzing]]
- [[Gunyah Hypervisor Fuzzing]]
- [[Sub Project - Video Driver security assessment]]





