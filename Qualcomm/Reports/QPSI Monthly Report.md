---
up: "[[Qualcomm MoC]]"
tags:
  - "#qpsi/report"
status: in-progress
created-date: 13-12-2024
summary: Monthly Report which is shared with either org
---

# Active Projects

## Windows on Snapdragon [[WoS]] Fuzzing

**Project Description** : Windows Driver Fuzzing – Device IOCTL fuzzing (Munawwar Hussain Shelia): The objective of this project is to create a fuzzer specifically designed for testing device drivers that provide an IOCTL command interface. The fuzzer initiates its process by thoroughly examining the driver’s interface that is accessible from user-mode (UMD), particularly focusing on identifying the IOCTL Command IDs and associated buffers. Utilizing this gathered data, the fuzzer then proceeds to test the driver’s resilience by bombarding it with a buffer filled with random data. Additionally, the program is equipped with capabilities to craft and implement customized, structure-aware, and stateful fuzzing scenarios to enhance the testing process. 

### Update of the Month

- March
- April
- May
- June
	- on-hold
- May
	- Focusing on Fuzzing WiFi Driver IOCTL Interface. To goal is to get deeper code coverage so that bugs can be uncovered from that part. So far, I have found 9 bugs and filed corresponding CRs.
- April
	- Project started
- March
- Feb
- Jan

### Blocker

- June
	- Tech team not fixing the bugs reported in fuzzing campaign. 

## Fuzzing Android Kernel Driver with Syzkaller

### Project Description

We’re working to enhance the security of Android Kernel Driver by improving the Syzkaller description. The existing code coverage isn't great because the syz-description hasn’t been updated in a while. As code are added/removed to/from the driver, these updates need to be integrated into syzkaller description. To address this, each driver needs to be audited to identify outdated syz-descriptions and update it. Also different driver has different method of communication with user-space, so identifying attack surface and find appropriate method to integrate them in the syzkaller. Some driver using special method to communicate with the driver which complements IOCTL interface this also need to be integrated in the syzkaller.

### Update of the Month

- June 2024
	- The local fuzzing setup for the Pakala Device is now complete. The focus is on fuzzing the Camera driver, selected due to the 20 incident report tickets raised for it. The driver’s complexity is notably high, as the camera involves intricate hardware state transitions, resulting in a very stateful driver. During entry point analysis, approximately 136 IOCTL commands were identified for this driver. Currently, the TPG sub-system of the Camera driver is under audit, and as the project progresses, a syzkaller description will be added for it.
	- Passed this update to Nagaraju but he didn't update the report
- July
	- Local fuzzing setup for the Pakala Device is completed. The focus is on fuzzing the Camera driver, this driver selected due recent surge of incidents report. The driver’s complexity is notably high, as the camera involves intricate hardware state transitions, resulting in a very stateful driver. During entry point analysis, approximately 136 IOCTL commands were identified for this driver. Currently, the TPG & Actuator sub-system of the Camera driver is under audit, and as the audit progresses, syzkaller description will be added for these sub-system.
- August
	- Completed auditing & creating syzkaller description for TPG, Actuator, EEPROM, Flash, OIS sub-system. Also merged this work with Linux Security Team’s upstream repo.
- September
	- Completed auditing & creating syzkaller description for Sensor, CPAS and CSIPHY sub-system. Also merged this work with Linux Security Team’s upstream repo.
- Oct
- Nov
- Dec
- J

# Archived Projects

> Project which I am no longer working on


## Automotive HGSL Graphics Driver Review