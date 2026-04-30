---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/prompt-idea"
created-date: 2026-04-23
related:
summary:
---

## Notes

Approach this like a systems thinker.

1. Define the core problem clearly
2. Identify assumptions
3. List constraints and unknowns
4. Break into sub-problems
5. Propose 3 different approaches
6. Compare tradeoffs
7. Choose best approach
8. Give step-by-step execution
9. Highlight failure points
10. Suggest improvements after v1

Problem: I want to design a fault based instrumentation tool for mips 32 architecture for linux os. 

you can use the following article for reference [https://devblogs.microsoft.com/oldnewthing/20180416-00/?p=98515](https://devblogs.microsoft.com/oldnewthing/20180416-00/?p=98515). 

One very important feature i want to start with is tracing basic block, with all types of branch instructions, direct and indirect. The target program will be in running in multi-threaded environment. Discovering accurate CFG during runtime is very difficult problem.

I was think using  In-Process Branch Patching with Software Emulation, in the approach the instruction before the branch is patched with fault instruction, and re-allocated to different section and is as additional instruction which re-directs it back to it next instruction at location which it was patched. This we are not disturbing the delay slot semantics and do guess the next block can we done by based the instruction can be done. But i don't know what is the complexity of this implementation? Guessing the blocks based on indirect branch is very ticky, how is frida-dbi handling this?


- Our main goal is to do program tracing at basic block granularity. We start doing CFG using dynamic disassembly as illustrated in v1_plan.md.
- Use capstone to do recursive disassembly and place breakpoint on all of the breakpoints.
- Our biggest challenge is to find branch target from in-direct branching. 
- use shaman-breakpoint infra using pwrite syscall to write multiple breakpoints in one syscalls.
- I want you to use the recursive 
- 'BreakpointHandler' should have the breakpoint-start and temporary breakpoint metadata.


- Take the hybrid approach of stragery of 