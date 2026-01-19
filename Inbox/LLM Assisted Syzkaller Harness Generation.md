---
up: "[[Qualcomm MoC]]"
tags:
  - "#qpsi/project"
  - type/pinned-docs
created-date: 2026-01-14
status: in-progress
related: "[[Android Kernel Driver Fuzzing]]"
summary: Using LLM to generate Syzkaller harness
participants:
---

## Objective

## Task

1. [ ] Discovering Entry-points
	1. [ ] Validate the entry point with 
2. [ ] Analyzing IOCTL entry-point to generate Syzkaller grammar
	1. [ ] validate the grammar by compiling it and providing feedback to the model
3. [ ] Generate the syzprog to validate the grammar.
	1. [ ] model will execute the prog and collect the coverage feedback
		1. Coverage feedback can be collected from
			1. syzprog -cover parameter
			2. adb logcat
			3. ftrace

