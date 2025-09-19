---
tags:
  - "#type/reading/book"
status: todo
author: 
title: 
up: "[[Reading MoC]]"
created-date: 2024-12-14
completed-date: 2024-12-14
summary:
---

## Reference Notes

1. Dynamic Disassembly
	1. Rich set of runtime information at its disposal, such as concrete register and memory contents.
	2. The main downside of this approach is the code coverage problem: the fact that dynamic disassemblers don’t see all instructions but only those they execute. 
2. Disassembly Technique ( Chapter 8)
	1. **Linear Disassembly** - It iterates through all code segments in a binary, decoding all bytes consecutively and parsing them into a list of instructions. Many simple disassemblers.
		1. Disadvantage
			1.  intersperse data such as jump tables with the code, without leaving any clues as to where exactly that data is. 
			2. If disassemblers accidentally parse this inline data as code, they may encounter invalid opcodes. Even worse, the data bytes may coincidentally correspond to valid opcodes, leading the disassembler to output bogus instructions
			3.  ISAs with variable-length opcodes, such as x86, inline data may even cause the disassembler to become desynchronized with respect to the true instruction stream
	2. **Recursive Disassembly** -  recursive disassembly is sensitive to control flow. It starts from known entry points into the binary (such as the main entry point and exported function symbols) and from there recursively follows control flow (such as jumps and calls) to discover code. This allows recursive disassembly to work around data bytes in all but a handful of corner cases
		1. he downside of this approach is that not all control flow is so easy to follow. For instance, it’s often difficult, if not impossible, to statically figure out the possible targets of indirect jumps or calls.
		2. Challenges
			1. jump table in switch case implementation
			2. multiple ret statement in case statement block
			3. function that throws error and never returns.
			4. 

---

## Review


---

## Program Analysis

This section discusses concepts like [[Back Reference]], [[Forward Reference]]

- OSPREY: Recovery of Variable and Data Structure via Probabilistic Analysis for Stripped Binary