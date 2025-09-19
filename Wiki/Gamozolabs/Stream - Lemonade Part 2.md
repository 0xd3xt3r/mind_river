---
up: "[[Learning MoC]]"
tags:
  - "#type/video-stream"
status: todo
created-date: 2024-12-08
summary: Writing custom debugger in Linux
media_links: https://www.youtube.com/watch?v=6topgTC3-5k
---

Objective - hook the dynamic loader to be notified if new library is been loaded

## Annotations

1. [1:18:00]() - objective
2. [1:42:00]() - start of uselessness
3. [2:03:40]() - reading old code
4. [2:20:14]() - Objective of the Stream
	1. To put breakpoints we need to get notified when new module is loaded and for that tracing via syscall is not feasible.
	2. Instead we take [R_DEBUG Protocol](https://sourceware.org/pipermail/gdb/2000-April/004509.html) - apply breakpoint to monitor for library loading
5. [2:40:40]() - ignore
6. [2:40:40]() - starts digging into binary loading notification
7. [2:42:00]() - used DT_DEBUG notify shared library mapping in memory
8. [3:34:00]() - navigating glibc loader source. to understand DT_DEBUG manipulation
9. [3:42:00]() - function to hook \_dl_debug_state
10. [3:51:00]() - ignore
11. [4:00:00]() - continue
12. [4:20:00]() - elf header parsing

## Notes

- Find DT_DEBUG in the dynamic loader
	- This entry in the dynamic loader is filled with the run-time address of the r_debug structure
	- elf_setup_debug_entry - function which write to this address 
- /proc/pid/auxv - This  contains the contents of the ELF interpreter information passed to the process at exec time
	- /usr/include/linux/auxvec.h
	- We care about AT_PHDR and AT_ENTRY also interpeter base(to put elf loading breakpoint)
- The r_debug structure was not initialzed on EXEC ptrace event and we need a way to be notified the address is propulated.
	- glibc provides a function call \_dl_debug_state which is specify the be put breakpoint to get the elf load events. gdb uses this technique
	- lldb puts a breakpoint on \_rtld\_debug\_state this symbols are found in dynamic linker /lib/ld-linux-x86-64.so.
	- ld-uClibc - has \_dl_debug_state symbol
	- dynsym (dynamic symbols) - is always loaded in the process image
	- ![[gamozo-elf-parser.png]]

## Task

- [ ] Complete watching "Linux Lemonade - Part 2" stream #learning ➕ 2024-12-08