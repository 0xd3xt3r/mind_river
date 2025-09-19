---
tags:
  - type/writing/social-media
status: published
created-date: 10-12-2024
---
## Post #4 - Challenges IoT Embedded Reversing

While grapling with the challenges of reversing embedded binary I came across this tool.

Reverse engineering embedded binaries can be really challenging, while you have tools like IDA Pro, Ghidra, etc to do static analysis, but setting a dynamic analysis environment means you need to have access to device shell and get gdb binary into the device. While you don't have very good dynamic analysis tools like Intel Pintool, Frida, etc. Binary is executing in some fancy architecture which doesn't have all the DBD(dynamic binary analysis) in a very constrained environment where getting a debugger running itself becomes a challenge let alone running a tracing tools. The solution to many of these problem is emulation and a prominent tool in emulation space is QEMU, which is a cross platform emulation tool, so basically you can run a ARM or MIPS binary on x86_64 systems.

Qemu is not a complete solution to all the problems which I mentioned above and it won't run any and all the binaries out of the box. Not because its not capable of running the binaries of different platforms, but because the binary is interacting with a hardware and you may or may not have a emulator for that hardware. Good news! Qemu is open-source and has many different type of plugin support to write hardware emulation and other type of dynamic analysis related stuff. So its a ideal platform to invest your time and effort if you are into embedded reversing and vulnerability research.

If you are reversing a user-space binary of a different architecture you can use user-mode Qemu to execute a program and you can attach gdb to it to that process and have a basic debugging support. But can we do something more fancy? gdb is just too much manual work! We lets take a concrete example of program execution tracing.