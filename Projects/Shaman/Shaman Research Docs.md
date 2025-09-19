---
up: "[[Project Shaman]]"
tags:
  - "#type/sub-project"
  - "#status/active"
aliases:
  - shaman
status: todo
related: 
created-date: 2024-12-21
---

> **Summary**:: Project Roadmap and Research Docs

## Tasks

- [ ] Example Documentation #task #shaman ➕ 2024-12-08 📅 2024-12-08
- [ ] Code Coverage Feature Documentation #task #shaman ➕ 2024-12-08

## Abstract

Shaman is a platform-independent Dynamic Binary Analysis Framework designed to instrument programs without needing to recompile them or access their source code. It currently supports Linux (x86_64, ARM, ARM64) and Android (ARM64).

Think of it as a high-performance, scriptable debugger that can pause a program at any point to inspect or modify its memory and registers. This functionality enables tasks like tracing or altering System Call parameter, Injecting System calls, Collecting binary code-coverage, and intercepting or modifying function parameters.

The framework aims to simplify writing plugins and make it fast and easy to support new platforms, such as RISC-V, Power PC, MIPS, etc.

## Feature Pipeline

1. **Module Loading Event** - when ever there is a library been loaded you should get an event callback.
2. **Process Events** - in Resource Tracing features
3. **Signal/Exception handling callback**
4. **Loading library in the Tracee process** - this can be used load library in the tracee process and used to hook the function if needed.
5. **Instrumentation API** - as described in scorpio framework.
6. **Function Tracing API** - hooking function entry point and exit point to intercept parameter and return value just like system call tracing.
7. Implement GDB Remote protocol
	1. https://www.embecosm.com/appnotes/ean4/embecosm-howto-rsp-server-ean4-issue-2.html
	2. https://github.com/NationalSecurityAgency/ghidra/discussions/4033
	3. [Implementing GDB Stub Protocol](https://medium.com/swlh/implement-gdb-remote-debug-protocol-stub-from-scratch-1-a6ab2015bfc5)

## Challenges

1. Fetching children event is not consistent across Linux kernel version.

```bash
/**
* It is possible that this PID is not under our management,
* so now we have to check if any of our tracees have an
* event for us. We are no longer going to receieve signal from
* the `waitid()` as that may infinitely give us the same PID.
* However, the PID that it is reporting could be a child from
* a `fork()` event from one of our debuggees, and to get that
* `fork()` event we have to "look ahead" in the signal queue
* by non-blocking checking for signals from all of our
* tracees.
*
* This is designed for the very real case as such:
*
* signal_queue = [
*    STOP event from new child,
*    TRAP event from tracee telling us it forked the child,
* ]
*
* If we do not do this look ahead, the debugger will just
* infinitely loop on `waitid()` and never observe a PID
* under our control.
*
* It is also possible that we got a spurious `waitid()`
* indicating the PID of _another_ [`Debugger`] instance had
* an event. In this case, none of our tracees will have a
* signal, and we'll just go back to `waitid()` without doing
* anything. There's non-zero overhead to this, but I don't
* think there is any other way to implement this without pidfd
* which we are not using as it is too modern of a feature for
* the targets we want to support.
*/
```

## Research Papers

1. [An In-Depth Analysis of Disassembly on Full-Scale x86/x64 Binaries](https://www.usenix.org/system/files/conference/usenixsecurity16/sec16_paper_andriesse.pdf)
2. ARMore: pushing Love Back into binaries
	1. This paper focues on static rewriting of ARM binaries
	1. It can do this for PIC and non-PIC code with/out symbols
3. [SaBRe: load-time selective binary rewriting](https://link.springer.com/content/pdf/10.1007/s10009-021-00644-w.pdf?pdf=button)
	1. SaBRe rewrites specific constructs - particularly system calls and functions - when the program is loaded into memory, and intercepts them using plugins through a simple API.
	1. We also discuss the theoretical underpinnings of disassembling and rewriting.
	1. We developed two backends - for x86_64 and RISC-V - which were used to implement three plugins: a fast system call tracer, a multi-version executor, and a fault injector.
	1. Our evaluation shows that SaBRe imposes little overhead, typically below 3%.
4. [Non-stop Multi-threaded Debugging in GDB](https://s3.amazonaws.com/arena-attachments/309033/6f46f21a0abfe4de8f56468953378dfb.pdf)
  1. This paper accurately describes the challenges of breakpoint management in muli-threading environment. we have to implement these feature as gdb has implemented it.
5. Efficient, sensitivity resistant binary instrumentation
	1. http://www.dyninst.org/
	2. https://github.com/dyninst/dyninst
6. Anywhere, Any-Time Binary Instrumentation


## Ref

List of reference Material which will be helpful to buld the this tool

1. [ARM and MIPS Ptrace Impl](https://github.com/aleden/ptracetricks/blob/main/ptracetricks.cpp)
2. [Writing Debugger in CPP](https://blog.tartanllama.xyz/writing-a-linux-debugger-source-signal/)
3. [Various Qemu Build Linux Machine to test the tool](https://people.debian.org/~aurel32/qemu/)
4. [TinyInst - lightweight DBI Framework by googleprojectzero](https://github.com/googleprojectzero/TinyInst/)**
5. [Python3 with ghidra - Mandiant Project](https://github.com/mandiant/Ghidrathon)
6. [bcov produces coverage information without recompiling a program by instrumenting it with breakpoints](https://bcov.sourceforge.net/)
7. [Programming Design Patterns](https://gameprogrammingpatterns.com/contents.html)
  1. Design Pattern which might be helpful for out framework
8. [RR Debugger Technical Details](https://arxiv.org/pdf/1705.05937.pdf)
9. [RR Debugger Project](https://rr-project.org/)
10. [Gamazolabs - Linux Debugger part 2](https://www.youtube.com/watch?v=6topgTC3-5k&ab_channel=gamozolabs)
11. https://suchakra.wordpress.com/tag/dynamic-instrumentation/