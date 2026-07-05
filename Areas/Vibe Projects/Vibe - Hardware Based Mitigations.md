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

Explain this from first principles.

- Start with the most fundamental concepts.
- Build upward layer by layer.
- Avoid analogies initially.
- Define terminology clearly.
- Show how each layer connects.
- Then provide mental model.
- Then show real-world application.
- Then give common misconceptions.
- Then summarize in 5 bullet points.

Idea:

- Intel CET Shadow Stack and IBT are two control flow based mitigation which are used to ensure forward-edge and backward-edge mitigations.
- I want you to write a blogpost covering various aspect of the mitigation
- I want to you list all the instruction which are used in this feature along with an example on how to use these instructions.
	- list of instruction which are used in user-space
	- list of instruction which are only available in kernel space
	- which instructions are used to check if CET is enabled.
	- create a sample program and show the disassembly of what the instruction looks like.
- Cover different use-cases like
	- how to Forward edge is configured and how hash value is placed and verified
	- how the shadow stack is pop is verified and how does a failure occur. 
- I also want you to cover following use-case
	- how the shadow stack page is allocate and setup is done during the boot stage for kernel
	- how is shadow stack allocated when new process is lunched and how different threads maintain and switch their shadow stack.
	- What are different configuration can be setup and explain various configuration for different level of security policy.
- Explain how the mitigations is used for kernel and user-space. I want you to cover the following scenarios
	- how the user-space shadow stack space configured and how multiple threads executed and switch between the various threads.
	- How does the kernel execution setups its shadow stack space and switch between kernel threads, and exceptions and syscalls.
- I want you to also cover various threat models considered for attacking the shadow stack?
	- What is token in the shadow stack, how does it ensure the safety? Why can't the attacker groom a heap memory to replicate the setup?
	- why can kernel exploit creates its own shadow stack and create a fake shadow stack?
- What is the programming model for the shadow stack/IBT for kernel and user-space?

### Design
