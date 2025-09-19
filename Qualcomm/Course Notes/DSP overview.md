---
up: "[[Qualcomm MoC]]"
tags:
  - "#type/qcom"
summary: DSP sub-system overview by jyostna and Abhash
---

## DSP Overview Session : Part 1 (Jyotsna)

- QuRT privilege level
	- uKernel / Micro Kernel / Monitor Mode - exposes minimal functionality once compromised nothing can be done
	- Root PD / Guest Mode - Majority of the OS service reside here and different sub-system will have different driver in this mode for example Audio DSP with have different set of drivers compared to Modem driver.
		- Driver exposes IOCTL or QDI.
		- Many major issue were reported here after which many refactor took place
	- User Mode / User PD - least privilage code
		- can directly call micro-kernel which can then forward request to Root PD to service it. 
- SMMU in DSP - smmu faults are not handled by Kernel and passed to Apps and device restarts.
	- SMMU has SID, co-relates to user Process and it is used to enforce memory permission
- IPC / RPC
	- FastRPC - loads external code and execute it. its only on DSP
- Compute DSP / NSP / Turing - Compute intensive task from HLOS 
	- ![[Pasted image 20241016162048.png]]
	- Use case mostly Machine learning, Face Auth, finger print Auth.
	- OpenDSP initiative
	- Dynamic PDs - FastRPC shell
	- Hana and before only signed code used to run.
	- Signed used Corg Certificate
	- Unsigned apps can now run on DSP, sandbox from 125 syscalls to 26 only.
		- cannot make QDI/IOCTL call to RootPD.
	- Signed PD have access to more syscalls.
		- takes more time to load (300ms more)
	- Whitelist of process who can launch Signed PD in DSP (checkpoint incident mitigation) i.e. to reduce the attack surface.
	- Camera has access to more drivers in root PD
	- Signed PD are loaded by the HLOS but the Driver and DSP Kernel is in S2 memory

## DSP Overview Session : Part 2 (Jyotsna)