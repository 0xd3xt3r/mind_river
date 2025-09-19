---
tags:
  - Qualcomm
  - Windows
  - fuzzing
  - qpsi/project
  - kernel
  - "#type/sub-project"
  - "#status/active"
aliases:
  - windows kernel fuzzing
  - WoS Kernel Driver Fuzzing
status: done
up: "[[WoS Kernel Driver Fuzzing]]"
related: 
created-date: 2024-12-14
summary: Fuzzing windows IOCTL Interface
---

> **Summary**:: 

# Fuzzing IOCTL Interface - Part 2

## Objective

- Fuzzing Qualcomm's Windows Kernel Driver IOCTL Interface
- Driver Fuzzing Pipeline
	1. Bluetooth 
	2. Camera

## Notes

- WoS Fuzzing Campaign lead by Nicolas
- Build Info:
	- First Build ID : APSS.WP_HA.1.0-05917-SC8380XAAASFNWAA-1
	- New Build with Ashish help : APSS.WP_HA.1.0-06556-SC8380XAAASFNWAA-1
- [Docs Update Link](https://confluence.qualcomm.com/confluence/display/WOSSEC/WoS-Sec+Opened+CRs)

### IOCTL Interface Fuzzing

#### Bugs 

WoS Fuzzing Campaign Bugs

| CR | Description |
|---|---|
| 3752770 | Battery Management System |
| 3752780 | Battery Management System |
| 3752785 | Bluetooth |
| 3758953 | Bluetooth |
| 3764804 | WiFi bug |
| 3764816 | WiFi bug |
| 3764820 | PMIC Battery management system |
| 3779989 | DSP FastRPC |

## WiFi Bugs

### Bug Tracker

| ID | CR | Tag | Comment |
| --- | --- |---|---|
| 1 | 3804383 | valid | crash_16 - 185370bd-e530-485b-ba71-4e260df5e1f8, can't reproduce the CR |
| 2 | 3804384 | valid dos | crash_13 - 3e6d52d3-7bdd-4dbb-a89e-aa7679d65ec4, f397e947-19d7-4750-8566-3d1a9de42a00  |
| 3 | 3804398 | valid | need more clarity, crash 11 - 657d89e7-7fe7-4c9d-84f6-46dc5e6216b6, 89e3f5f9-6f3d-4811-8874-609941b79574, abbbcd3d-60de-4577-94ce-ab207c740743, d2dea889-50e9-46ad-9c3a-c0433baefc0e |
| 4 | 3804424 | valid | crash 15 - c73c4b4a-2cf3-4fe8-8ae3-d3221d390996 |
| 5 | 3817188 | valid| Child of 3818037 bug \#11 which is not true|
| 6 | 3817212 | valid| Child of 3818037 bug |
| 7 | 3817216 | valid| don't know how to handle variable size buffer|
| 8 | 3817233 | valid | |
| 9 | 3817238 | valid | |
|10 | 3817243 | valid | don't know how to handle variable size buffer|
|11 | 3818037 | valid | parent of 3817212 3817188, not true|
|12 | 3818109 | valid | |
|13 | 3818118 | valid | |

---

## Bluetooth Driver 

### Bug Tracker

| ID | CR | Category | Comment |
|---|---|---|---|
| 1 | 01 | #bug-group| What happened?|

---
