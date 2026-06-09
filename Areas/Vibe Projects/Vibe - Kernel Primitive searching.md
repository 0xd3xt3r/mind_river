---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/vibe-specs"
created-date: 2026-06-03
related:
summary: Search for memory grooming primitives to for Linux Kernel
---

## Kernel Primitive searching

### Description

Memory grooming primitives are been killed as the kernel evolves. We can create a program which to 

### Design

1. Do the build system instrumentation to capture the build commands. This will be later used for macro expansion.
2. use https://github.com/trailofbits/trailmark to create the source code graphs.
3. create entry point file for trailmark. The entry points are the all the system call interface which are exposed to the user-space.
4. Then create different queries to find the primitives from the user-space.
5. There are different  type of primitives
	1. Fixed size primitives 

### Acceptance Criteria
- Criterion 1
- 
### Priority
2
### Type
epic

### Assignee  
ross

### Labels
label1, label2

### Dependencies
