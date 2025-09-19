---
tags:
  - "#fuzzing"
  - "#reverse-engineering"
  - "#reading/research-paper"
up: "[[Research Paper MoC]]"
status: todo
created-date: 2024-12-16
---

> **summary**:: All Research Paper i have read so far

[[SoK- The Progress, Challenges, and Perspectives of Directed Greybox Fuzzing]]

## LibAFL QEMU : A Library for Fuzzing-oriented Emulation

- growing number of forks of QEMU that are hard to maintain up to date with upstream.
- a library written in Rust that wraps QEMU as a static library with most of the codebase for instrumentation and fuzzing written out of the main source tree of QEMU for ease of maintenance
- It offers a wide range of instrumentation options and fuzzing capabilities for both userspace and system emulation such as a powerful hooking system, a ready-to-use collection of instrumentation like AddressSanitizer for binaries and support for snapshot fuzzing
