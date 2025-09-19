---
up: "[[Tooling MoC]]"
tags:
  - "#type/tooling"
created-date: 2024-12-26
related: 
summary:
---

Below are some of the details of the internals of the gdb code. I started digging in the source code so that I can extract out the technique to implement my own tools [[Shaman Research Docs]]

# Source Code Reversing

## File Path 

| Source Path | Description |
|---|---|
| gdb/arch/arm.c | all the configuration details of arch architecture |
| gdb/arm-linux-nat.c | debugger related implementation of architecture specific code |
| gdb/arch32-linux-nat.c | same as a above but for x86 architecture |
| gdb/arm-linux-tdep.c | breakpoint instruction for ARM ISA |
| regcache.cc| storing and caching register information of all type GP/FP, etc |
| gdb/breakpoint.c | Implements all the breakpoint related functionality |
| waitstatus.h | implements process state-machine  |

## Identifier

Below are some of the function and classes Identifier which are important

| Identified Name | Identifier type| Description |
|---|---|---|
| fetch_registers/store_registers | functions |get/set the register value for thread using debugger API |
| arm_linux_nat_target | class | arm implementation of debugger implementation |
| x86_breakpoint | variable | breakpoint instruction for x86 ISA |
| target_desc | struct | target description of tracee |
| arm_breakpoint_at | function | insert ARM breakpoint |
| target_waitkind | enum | all the possible state of process |
| arm_canonicalize_syscall | function | maps from the native arm Linux set of syscall ids into a canonical set of syscall ids|
| aarch64_canonicalize_syscall | function | same as above for intel x64 architecture |
| arm_register_aliases | struct | register mapping for arm processor |
| aarch64_register_aliases | struct | register mapping for intel processor |


## Ref

1. [Non-stop Multi-threaded Debugging in GDB](https://s3.amazonaws.com/arena-attachments/309033/6f46f21a0abfe4de8f56468953378dfb.pdf)
2. [Good discussion on Templating in cpp](https://blog.feabhas.com/2014/06/template-inheritance/)