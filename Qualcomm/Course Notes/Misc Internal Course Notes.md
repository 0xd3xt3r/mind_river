---
tags:
- course
---

# Internal Course Notes

## 05-10-2021

- [[MSM Architecture]] and functional Overview
	- SoC design consumes very less power compared to Mother board design it is also called milli-watts design
	- Bus is needed to communicate between multiple Processor like Modem , application processor.
		- there are two bus, data and address bus.
	- Power management IC (PMIC) just like SMPS in PC.
		- there are power switches sub-system level.
		- power island is the group of sensor which operate on their own will like pedometer, it collects data and sends it to CPU and goes back collecting it.
			- power island work on their own they don't need CPU, Memory, DDR to work, all they need to work is power. 
			- has its own processor and can walk up other sub-system
	- ![[Pasted image 20211005155938.png]]
	- ![[Pasted image 20211006205216.png]]

## 06-10-2021

**[[SMEM]] Overview**

- share memory which is used for IPC between different sub-system like DSP, HLOS, etc
- access control information is stored in Table Of Content [[ToC]], which is stored at the end of SMEM memory.
- SMEM is partitioned in to different section and each section is used by different pairs of sub-system. ToC is used to enforce access control.
- there is two section is SMEM, static and dynamic heap.
- TCSR_WONCE register stores address of ToC info struct which used by other system to discover SMEM metadata. XBL will update this register
- all the above work is done by XBL	
- TZ provides hardware protection through XPU. Since hardware is expense one few sub-system use them like MPSS, SPSS and TZ. XPU is a hardware
- Hypervisor provides you with software protection
- there is a TOC header (\$TOC) and each partition also has header  (\$PRT).
- there are two list in the partition structure, cached data and un-cached data.
- [Reference Diagram](https://qwiki.qualcomm.com/qct-mpsw/Secure_SMEM_and_SMD_Partitioning)

## 07-10-2021
- 

