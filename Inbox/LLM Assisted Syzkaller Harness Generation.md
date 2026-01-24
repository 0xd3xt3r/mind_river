---
up: "[[Qualcomm MoC]]"
related: "[[Android Kernel Driver Fuzzing]]"
tags:
  - "#qpsi/project"
  - type/pinned-docs
created-date: 2026-01-14
status: in-progress
summary: Using LLM to generate Syzkaller harness
participants:
  - "[[Nilo]]"
  - "[[Hashir]]"
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

## Task

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


## Claude code skills

1. Identify entry points
2. 