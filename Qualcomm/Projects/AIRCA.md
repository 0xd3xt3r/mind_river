---
up: "[[Qualcomm MoC]]"
tags:
  - "#qpsi/project"
  - type/pinned-docs
  - qpsi/ar26
created-date: 2026-01-13
related: "[[Claude Code]]"
status: in-progress
summary: AI assisted Root Cause analysis tooling
participants:
  - "[[Nilo]]"
  - "[[Nicolas]]"
aliases:
  - AI Root Cause analysis
---

## Objective

Samsung wants to enable MTE or Kaanapali devices, which we did but it cause There are 7744 total crashes of which 52 are system crashes in user-mode and kernel-mode. The goal of this project is to do RCA on all these crashes and report it to the tech team for further analysis.

## Notes

- [ ] Tooling to analyze User and kernel crashes.
- [ ] AXIOM has the MTE build, it collects the crashes.
	- crash scope - its uses T32 as backend to create dump
	- Crashed when trying to load the large dump, dump is 12 GB.
		- The solution is to convert trace32 dump to gdb coredumb format and then load the dump using gdb multiarch.
- [ ] Create Tooling to triage user-mode and kernel-mode
	- [ ] Kernel crash is done
	- [ ] Userland crashes analysis is in progress.
- [ ] How to trigger the crash?
	- we don't need to trigger the crash, Jira ticket has all the necessary information like crash dump data loaded from Axiom.
- [ ] What is the expectation from Auto Triaging?
	- [ ] Generate report using crash
- [ ] How to setup the trace32?
	- Not needed, we are not attaching to to a live device.
- [ ] What the issues mentioned in the email by Murali.
- [ ] How to create MTE build for Kaanapali

## Task

- [ ] **Workflow**
	- [ ] Find all the Jira issues by Samsung
	- [ ] Trigger the claude with necessary information like
		- [ ] Jira ticket 
		- [ ] location of the crash dump
	- [ ] then LLM will do the root cause analysis and put the analysis result in the jira ticket comment
