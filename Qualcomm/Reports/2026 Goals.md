---
up: "[[Qualcomm MoC]]"
tags:
  - "#qpsi/report"
status: in-progress
created-date: 2026-01-09
summary: Goal planning for 2026
---

### Android Kernel Security

#### KCOV Coverage Improvement
- **Problem:** Currently Syzkaller fails to get depth coverage, one of the mail reasons I suspect is the lack of accurate coverage reporting by kcov. The primary reason for this is that coverage can be stored in limited buffer size, and there are large number of basic blocks which we fail to capture in the limited buffer size.
- **Solution:** 
	- One solution could be to reduce the number of basic blocks which are instrumented by kcov. This will reduce the BB to report and hence better coverage.
	- One of the ways to reduce the number of basic blocks is to only instrument function entry point. This way we can have accurate reports for function level coverage. Additionally, as a step further we can add instrumentation in section of code which could yield more bugs like for-loop or places where there is lot of memory manipulation.

#### Attack surface management for kernel driver

- **Problem:** Kernel drivers are exposing functionality to user-space via different interface. The interface has to be enumerated automatically, since the code keeps evolving, we need a way to automatically track the entry points. This data can also used later for code analysis in LLM.
- **Solution:** Linux kernel has defined various interfaces which the driver can implement to expose its functionality to the user-space. In this project we will write a code parser to extract these entry points.

#### LLM Assisted harness generation for Syzkaller

The goal of this project is to use LLM to improve the Syzkaller grammar. Although significant amount of efforts has been put to write the grammar manually, the kernel driver code keeps updating and the Syzkaller grammar falls behind these update as some also have to update the grammar to keep up with the new code. The project will explore the possibility if we can use LLM to update the Syzkaller grammar with new code.

### Out-of-Band Security Review

The goal of this project is to conduct a through security review of out-of-band firmware components which exposes a remote attack surface. There are also other third party components which critical to security of the firmware like network stack of Zephyr OS which also needs to be audited and fuzzed.
