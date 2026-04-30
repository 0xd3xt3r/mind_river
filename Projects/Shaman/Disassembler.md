---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/prompt-idea"
created-date: 2026-04-20
related:
summary: Dynamic Disassembly
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

Problem: Hybrid Disassembler - the primary goal of the project is to collect the execution trace of running process at basic-block granularity. I want to integrate this in custom debugger i am writing. The accuracy of the disassembly is very critical to the success of the project. We are writing whole program in pure C.
- In this disassembler you will have following:
	- Out-of-process
		- access to live state of the process, you can inspect the register, memory, or anything inside a process. 
		- We have to ability stop the process at any-point and inspect the process, this will be out-of-process debugger.
		- Has ptrace API access.
		- Give events like thread, library load events to in-process agent for further work.
	- In-Process agent
		- But there will be another piece of code inside the target process called in-process agent which can run anything we want.
		- The trace data will be emitted to ring buffer. Which will be collected by our-of-process debugger.
		- Our debugger agent will be in the same process which we are disassembling, and we can halt anywhere we want.
		- it will have capstone sdk in it so it can do instruction decoding. All the disassembly logic should be here.
	- we will have fault based stop approach for in-process debugging and debugger process  as fallback.
- This is strictly going to be user-space process.
- When the process and you attach to a process you can read the current executing instruction and start disassembling from that instruction, if a new process is spawned the entry point of the becomes the seed of disassembly.
- Basic Block discovery algorithm
	- Start disassembly and discover new basic blocks by using recursive disassembly, this way you will start discovering the graph from the entry-point, but there will be dead-end like indirect call instruction, because they are difficult to predict and only available at run-time. We can stop the process there and inspect the process.
	- Once we reach dead-end points we can gain further clarity and start the same disassembly recursively and discover new basic-blocks and dead-ends. We will deal with new dead-end in similar manner on how we dealt with old dead-end. We keep doing this until the program completes it execution.
- Don't use any platform/ABI/Hardware dependent approach, for disassembly I want you to use capstone engine.
- The target :
	- will be multi-threaded.
	- symbols will be stripped 
	- has ASLR support and all modern mitigations.
	- could be written in programming language like c,c++, rust, go. So think about indirect type-aware indirect solution.
	- Architecture x86_64, arm32, thumb, arm64, mips32, mips64.

We are purely in planning phase, don't write any code.



## Approach - thinking

Act as a critical thinking partner.

1. Do not agree blindly.
2. Challenge assumptions.
3. Point out weak logic.
4. Suggest alternatives.
5. Identify risks.
6. Improve the idea.
7. Propose better direction.

Idea: 