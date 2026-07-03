---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/vibe-specs"
created-date: 2026-06-24
related:
summary:
---

## Hardware Based Mitigation

### Description

- Intel CET Shadow Stack and IBT are two control flow based mitigation which are used to ensure forward-edge and backward-edge mitigations
- I want you to write a small blogpost covering various aspect of the mitigation
- I want to you list all the instruction which are used in this feature along with an example on how to use the instruction
- I also want you to coverage how the shadow stack page is allocate and setup is done during the bootstage and who different configuration can be setup and explain various configuration for different level of security policy.
- Explain how the mitigations is used for kernel and user-space. I want you to cover the following scenarios
	- how the userspace shadow stack space configured and how multiple threads executed and switch between the various threads.
	- How does the kernel execution setups its shadow stack space and switch between kernel threads, and exceptions and syscalls.
- I want you to also cover various threat models considered for attacking the shadow stack?
	- What is token in the shadow stack, how does it ensure the safety? Why can't the attacker groom a heap memory to replicate the setup?
	- why can kernel exploit creates its own shadow stack and create a fake shadow stack?
- What is the programming model for the shadow stack/IBT ?

### Design
