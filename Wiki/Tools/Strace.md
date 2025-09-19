---
up: "[[Tooling MoC]]"
tags:
  - "#type/tooling"
created-date: 2024-12-26
related: 
summary:
---

## Source Code Walk through

- `struct tcb` is the struct which stores the system call number along with the its parameter and once after executing the system call it also stores its return value **tcb** stands for trace control block
- `struct sysen` - this struct stores all the system call handler function pointer. The function pointer is stored in **sys_func** function
- `scno_is_valid(kernel_ulong_t scno)` - Checks whether scno(system call number ) is not out of range, its corresponding `sysent[scno].sys_func` is non-NULL, and its `sysent[scno].sys_flags` has no `TRACE_INDIRECT_SUBCALL` flag set.
-  macro `SYS_FUNC` define system call handling functions for eg for read `SYS_FUNC(read)` system call will create function `int sys_read(struct tcb *tcp)`.
- Each Linux sub-system is very well decoded by strace and the decoder handler are present in individual file for eg I/O sub-system syscall handler are present in io.c there are other handlers like bpf.c, ioctl.c, etc. 
	- ioctl.c decode ioctl functions call data structures.
	- bpf.c had code which decode instructions
	- io.c has all the code for the read/write etc related decoding
- config.h file has all the compilation configuration you can edit this file to enable and disable certain features.
- strace.c - the main entry point file
- syscall.c - system call handling code
	- tamper_with_syscall_entering
	- tamper_with_syscall_exiting
	- `syscall_entering_decode`
	- `syscall_entering_tracing`  - this function is call before system call is actually made. Tampering function `tamper_with_syscall_entering` is later call in this function. 
	- `syscall_entering_finish`
	- `syscall_entering_tracing` - this function is call after system call is actually made. Tampering function `tamper_with_syscall_exiting` is later call in this function.

