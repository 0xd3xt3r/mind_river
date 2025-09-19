---
up: "[[Learning MoC]]"
tags:
  - "#OST"
  - "#UEFI"
  - "#firmware"
  - "#learning/course"
title: Architecture 4021- Introductory UEFI
link: https://p.ost2.fyi/courses/course-v1:OpenSecurityTraining2+4021_Intro_UEFI+2022_v1
status: todo
summary: UEFI course from OST
---

## Notes

- There are multiple options for booting like coreboot and UEFI, etc.


## Firmware Fundamentals

- [[Firmware]] is a software which is :
	- considered by users which is integral part of hardware
	- lowest level of software provided for hardware initialization
	- critical [[hardware initialization]] which OS can't do or get involved in
- Firmware is usually shored in [[SPI]] memory on mainboard
- Firmware which loads OS is called bootloader which is typically shored in boot rom which is read-only memory
![[Pasted image 20230103144503.png]] 