---
created-date: 2025-01-08T23:00
up: "[[Meeting MoC]]"
presented-by:
participants: "[[Scott]]"
related: "[[Qualcomm/Projects/Camera Driver Fuzzing]]"
tags:
  - "#type/meeting"
  - "#qpsi/meeting"
---

> **summary**:: Some Bugs which I have figured by code review

## Meeting Minutes

- [x] #task [[Qualcomm/Projects/Camera Driver Fuzzing]] Discuss from Issue 17 - 24
	- cam_packet_util_dump_patch_info check if patched
- [ ] #log/sprint  [[Qualcomm/Projects/Camera Driver Fuzzing]] : Syzkaller Coverage issue
	- coverage is overflowing from the buffer, that the reason its not able to report the coverage for certain basic block
	- the syzkaller seed scheduler is not going deep to test 3 4 layer down the call chain.

## Action Item