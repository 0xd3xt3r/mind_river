---
up: "[[Qualcomm MoC]]"
related: "[[Android Kernel Driver Fuzzing]]"
tags:
  - "#qpsi/project"
  - type/pinned-docs
created-date: 2026-01-14
status: in-progress
summary: Using LLM to generate Syzkaller harnesses
participants:
  - "[[Nilo]]"
  - "[[Hashir]]"
aliases:
  - claude fuzzing
---

## Objective

- We want to have find scalable method of manage security of kernel driver.
- 75% internal bug detection.


## Current Problems / Challenges

1. How do we track the change in the code base, and how does the new change affect the security posture?
	1. How do we track new changes to the kernel driver?
	2. How can we associate the change with an entry-point?
	3. Finding the outdated struct or function grammar.
	4. Parse the structure from the Syzkaller grammar and track changes in source code.
		1. The two way relationship, 
			1. grammar to source - use analyst expertise to track source change
			2. source to grammar
	5. This tool will be called as *Attack Surface Management*.
	6. Revie Goal we should know the risk, may be might not able 
2. How can we improve the corpus and coverage?
	1. Coverage is currently been under-reported?
	2. Syzkaller is not doing depth first search?
	3. Trace the output of current unit-test cases and covert that to syz-prog corpus.
3. How can we scale the Syzkaller grammar change?
	1. This problem relates to 1st problem, find the entry point and see how to 
4. How can we use the reported CR's to enhance our security posture of the driver?
	1. Can we use this to add new pattern detection?
	2. Can we use this to improve coverage?
5. Device Farm management by Claude?
	1. Give some device for experimental configuration.
	2. Manager large farms of device to debug and report.
---

## Architecture

1. Entry-points identification
	1. **Importance**: Have an automated method to enumerate entry-point can reduce human-error and also have a comprehensive list of attack surface.
	2. [ ] Entry point discovery is done using pre-defined structures and parsing the target code with tree-sitter
		1. [ ] Limitation of this method is it relies solely on source code analysis. This could include code which can be remove from final binary.
			1. The reason we do this is because, integrating with build system can complicate the solution.
				1. We can use other method of entry point validation using binary analysis of the target driver
				2. Validate it by poking the target device.
			2. Pointer analysis - this problem is difficult to deal at source level analysis, be we can then take help of binary analysis done by Bernard's work.
		2. Parse kernel C code to find entry points
			1. Linux kernel has a well define interface to expose kernel driver to the user-space
			2. Use simple C parser library to identify those user-kernel entry points.
			3. One these entry-points may not be valid because we have not extracted this from binary. Integrating build system introduces additional complexity.
	3. **Validate the entry-point** with agent program which can run inside the device.
		1. [ ] This step will only find if the device path exist and 
	4. Combining both the previous step can help you to find the function in the kernel component which will be execute along with the user-space device path.
	5. The **end-goal** of this step is to have a device path which the user-space program can connect to and the corresponding function which will be executed in the kernel driver.
2. [ ] Analyzing IOCTL entry-point to generate Syzkaller grammar
	1. **Importance** : The step will help use understand where is the gap which we need to fill. What is the outdated grammar which corresponding kernel code does the grammar belongs to and what is the gap? Also fill the gap.
	2. Validate the grammar by compiling it Syzkaller and providing feedback to the model if there are any error.
	3. Asking the LLM to generate grammar for specific entry point and validate it. Use generation and validation in the loop.
3. [ ] Generate the syz-prog to validate the grammar.
	1. **Importance**: The reason to have this step is to validate if the new grammar has really added any new coverage?
	2. Generated syz-prog will also help us to have a baseline corpus.
	3. [ ] Model will execute the prog and collect the coverage feedback
		1. Coverage feedback can be collected from
			1. syzprog -cover parameter
			2. adb logcat
			3. ftrace
			4. kprobe

---

## Project proposal

[Android fuzzing Roadmap](https://qualcomm-my.sharepoint.com/:w:/r/personal/nredini_qti_qualcomm_com1/Documents/Documents/Android%20Kernel%20Security%20Projects%20and%20Roadmap.docx?d=wa7f3b6319b784154a2afcf5439628493&csf=1&web=1&e=gJXgiO)

### P1 - Attack surface management

- The goal of this project is to pin-point the changes that are coming the kernel driver and how does that affect the entry-point.
- For this we need to clone the git repo and build the symbol index for all the branches and store the data in either json file or sqlite-db. The stored file should have branch name and the git commit at which the scan was done.
- Input to the system will be git url
- Spec
	- All the code should be written in python
	- the code we will be scanning will be strictly C code for Linux kernel.
	- The index should store symbol and cross-reference information
	- update to a symbol will notified to a users
	- When there is a new commit try to map in which symbol the change has occured, like funcition 
Break down the task and store the work plan in 'beads' in the form of epic and task

#### Task

- [ ] Change management
	- [ ] Change wrt to Syzkaller metadata
		- [ ] Structure change
			- [ ] new field
			- [ ] change data type
	- [ ] Change wrt to commit-version
		- [ ] store the snapshot of the current scan
		- [ ] diff the change
	- [ ] link the change to the entry point via call graph
	- [ ] This type of structure
		- [ ] kernel has standard way of defining IOCTL command patterns. parsing that will help use extracting low hanging fruits
		  ```C
		#define IOCTL_KGSL_SUBMIT_COMMANDS _IOWR(KGSL_IOC_TYPE, 0x3D, struct kgsl_submit_commands)
		  ```
- [ ] Entry point management
	- [x] scan for all IOCTL methods

### P2 - Device Fuzzing Farm

#### Task

- [ ] Task assigned to [[Hashir]]
	- [ ] Create a dev environment
	- [ ] Setup 8 devices
	- [ ] Connect the all the device to a single syzkaller instance

### P3 - Coverage gap analysis

**Goal** - The goal of the project is to improve the current lag in the fuzzing coverage?
**Approach:**
1. Analyze the IRVR tickets
2. Understand the current gap in the Syzkaller grammar.
3. If the grammar is complete, investigate why is the Syzkaller not exploring the code path?

### Task

- [ ] Drivers for Ashish
	- [ ] FastRPC
- [ ] Drivers for Hashir
	- [ ] Camera
- [ ] Driver for Munawwar
	- [ ] KGSL

### P4 - Agentic code review

1. Create workflow that help you review the command bug patterns.
2. Review for function like `copy_to_user` and `copy_from_user`.
3. Create pattern by analyzing IRVR incidents.
4. Domain specific review
	1. In-case of camera function like `cam_mem_get_cpu_buf` and `cam_mem_get_io_buf`

---

1. Syzkaller to SFE Bridge
2. WoS Fuzzing Farm
3. UEFI Fuzzing
4. Nicolas
	1. Automation
	2. RCE for Android and Windows
		1. Automated Patch and verification (Require PoV)
	3. Infrastructure should be up and running
