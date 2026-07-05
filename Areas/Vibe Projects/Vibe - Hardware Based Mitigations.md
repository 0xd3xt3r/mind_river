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
	- I want you to explain what extract problem are they addressing.
- I want to you list all the instruction which are used in this feature along with an example on how to use these instructions.
	- list of instruction which are used in user-space
	- list of instruction which are only available in kernel space
	- which instructions are used to check if CET is enabled.
	- create a sample program and show the disassembly of what the instruction looks like.
- What is the programming model for user-space shadow stack and IBT , cover the following topics ?
	- How the shadow stack is pop is verified and how does a failure occur. 
	- Context Switching & Thread Switching
	- Dynamic Stack Unwinding / C++ Exceptions
	- Custom threading/co-routine
	- how is shadow stack allocated when new process is lunched and how different threads maintain and switch their shadow stack.
	- How to Forward edge is configured and how hash value is placed and verified. What does endbr64/endbr32 instruction does ?
		- Describe the caller-callee handshake where the caller preloads a Set ID (SID) hash into a register
- In this section we will explore supervisor shadow stack for kernel which following scenarios:
	- how the shadow stack page is allocate and setup is done during the boot stage for kernel
	- What are different configuration can be setup and explain various configuration for different level of security policy.
	- How does the kernel execution setups its shadow stack space and switch between kernel threads, and exceptions and syscalls.
- I want you to also cover various threat models considered for attacking the shadow stack?
	- If shadow stack offers read and write via only controlled instructions and disables only certain instruction it just about reducing attack surface? am i right about this? what am I missing?
	- What is token in the shadow stack, how does it ensure the safety? Why can't the attacker groom a heap memory to replicate ,the setup?
	- why can kernel exploit creates its own shadow stack and create a fake shadow stack?
- What is the programming model for the shadow stack/IBT for kernel and user-space?
- What are some of the successful attacks on this mitigation?
- Based on the our research so far I want create a 5 part series of blogpost which covers everything we have discussed so far in the discussion. Help me list what all aspect should be covered in each part. The first part should be an instruction of the CET and should be self contained reference. But the sub-sequent part will cover other aspect in great details

### Polished Prompt

Part 1: The Foundation—Intel Control-flow Enforcement Technology (CET)

**Goal:** Provide a self-contained reference on how modern hardware architecturally prevents control-flow hijacking.

- Explain the concept of forward edge and backward edge. How it have been manipulated by attacker?
- **The Threat Model:** Define **Return-Oriented Programming (ROP)** and **Jump/Call-Oriented Programming (JOP/COP)**, where attackers chain "gadgets" to execute arbitrary logic.
- **Shadow Stack (Backward-Edge):** Explain the second, hidden stack dedicated to return addresses. Detail the comparison mechanism: `CALL` pushes to both stacks, and `RET` compares them, triggering a **#CP Control Protection fault** if they differ.
- **Indirect Branch Tracking (IBT) (Forward-Edge):** Introduce the **ENDBR64** instruction, which acts as a "landing pad" for indirect jumps. Any indirect transfer not landing on this instruction triggers a fault.
- **Deterministic vs. Probabilistic Defense:** Explain why CET is an architectural law (deterministic) compared to defences like **ASLR** or **Stack Canaries**, which are probabilistic and bypassable through information leaks.
- I want to you list all the instruction which are used in this feature along with an example on how to use these instructions.
	- list of instruction which are used in user-space
	- list of instruction which are only available in kernel space
	- which instructions are used to check if CET is enabled.
	- create a sample program and show the disassembly of what the instruction looks like.
- What is the programming model for user-space shadow stack and IBT , cover the following topics ?
	- How the shadow stack is pop is verified and how does a failure occur. 
	- Context Switching & Thread Switching
	- Dynamic Stack Unwinding / C++ Exceptions
	- Custom threading/co-routine
	- how is shadow stack allocated when new process is lunched and how different threads maintain and switch their shadow stack.
	- How to Forward edge is configured and how hash value is placed and verified. What does endbr64/endbr32 instruction does?
- Write a sample program then show how to compile the program and disassemble it and explain the interpretation. 

Part 2: Precision CFI—FineIBT and System-Wide Hardening

**Goal:** Detail how the system moves from "coarse-grained" hardware constraints to "fine-grained" software-assisted integrity.

- **The Limitation of Coarse IBT:** Explain that standard IBT allows jumping to _any_ function starting with `ENDBR`, even if the signature is wrong.
- **FineIBT Mechanics:** Describe the **caller-callee handshake** where the caller preloads a **Set ID (SID)** hash into a register, and the callee verifies it immediately after the `ENDBR` instruction using a `sub-je` sequence.
- **Hardening the Pipeline (NOPout):** Discuss how **NOPout** elides unnecessary `ENDBR` instructions at load-time to further shrink the attack surface.
- **ABI Evolution:** Detail the changes to the **Procedure Linkage Table (PLT)** required to support cross-module calls without breaking CET protections.

Part 3: The Adversary—Bypasses, Attacks, and Side Channels

**Goal:** Critically analyze the architectural gaps and sophisticated techniques used to circumvent CET.

- **eBPF Subversion:** Analyze how **eBPF** bypasses compiled CFI by using a dynamic, bytecode-driven model to perform **Interp-flow Hijacking** or subverting the **Verifier** to achieve arbitrary memory access without touching C-code pointers.
- **Non-Control-Flow Attacks:** Introduce **Counterfeit Object-Oriented Programming (COOP)** and **Data-Oriented Programming (DOP)**, which utilize legitimate forward-edge calls or modify non-control variables to achieve malicious goals.
- **Administrative Gadgets:** Explain how specialized instructions like **WRSS** (Write to Shadow Stack), intended for JIT compilers, can be abused as "Super ROP gadgets" to manually forge return addresses.
- **Speculative Side Channels:** Address the **Spectre** threat, noting that FineIBT still leaves a small **speculative window (~15 bytes)** where gadgets might be transiently executed despite architectural blocks.

Part 4: The Programming Model—Supervisor Shadow Stacks (SSS)

**Goal:** Explore how the OS kernel manages shadow stacks and maintains integrity during context switches.

- **The Kernel Twist:** Explain **Supervisor Shadow Stacks (SSS)**, which protect Ring 0 code from kernel-mode ROP attacks.
- **Hardware Tokens:** Detail the **Shadow Stack Token**—a 64-bit descriptor embedding the stack's linear address and a **"Busy bit"** to prevent attackers from redirecting the stack pointer to fake memory.
- **Core Instructions:** Provide a technical guide to instructions like **RSTORSSP** (restore stack), **SAVEPREVSSP** (trace switches), and **INCSSP** (stack unwinding for exceptions).
- **Virtualization-Based Security (VBS):** Explain how a **Hypervisor (Ring -1)** can protect kernel page tables to ensure an already-compromised kernel cannot simply strip the Shadow Stack attributes from its own memory.

Part 5: Architectural Parallels—The ARM Response and Future Challenges

**Goal:** Compare Intel CET to ARM's security model and discuss the broader hurdles to mainstream adoption.

- **ARM Parallels:** Detail **Branch Target Identification (BTI)** as the equivalent to IBT and the newer **Guarded Control Stack (GCS)** as the deterministic equivalent to CET Shadow Stacks.
- **Pointer Authentication (PAC):** Explain ARM’s cryptographic approach to pointer integrity, which uses unused virtual address bits to store signatures.
- **PAC vs. MTE:** Contrast protecting control flow (PAC) with protecting data memory through **Memory Tagging Extension (MTE)** "color-coding".
- **The Mainstream Gap:** Based on the "SoK: Software Compartmentalization" findings, conclude by discussing why these fine-grained hardware tools are not yet the norm and the need for **holistic solutions** that consider policy, abstraction, and mechanism together