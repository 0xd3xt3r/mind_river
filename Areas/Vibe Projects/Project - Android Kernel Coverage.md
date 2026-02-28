---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/vibe-specs"
created-date: 2026-02-16
related:
summary: KCOV alternative to gather kernel coverage
---

## Android Kernel Coverage

### Description

I am currently using kcov for syscall tracking which is doing a good job, but the area which it is tracing is very huge (core kernel and drivers) this leads to coverage buffer been over-ridden with coverage. In my opinion, the best way to solve this problem is to reduce the number for probes which are installed to collect coverage. But there might be other solution which you can suggest.

### Design

Target Environment:
- Target: Android kernel driver in ARM64 architecture.
- Access: Source code, kernel compilation config, root via adb shell.
- Full control over target system

Driver Design:
1. It should be able to dynamically add and remove probe to the target driver.
2. The user should be able to change the granularity of the tracing either to function or basic-block level. Ignore in-line function tracing.
3. There will be different types of probe of basic-block or function tracing
	1. single hit and remove probe - this probe remove once it executed
	2. max 'n' probe - this probe is remove once it is executed 'n' number of time.
	3. never probe - this probe always sticks around no matter how many time it has hit.
4. Tracing function call-graph is very important and unique feature of this driver. Do research on how this can be implemented.
5. The execution will place a filter on the process for which all this tracing should be done. This is very important or else we are tracing code which is not triggered by us.
6. Since the kernel address range is know, the address of the executed driver can be compressed by removing the common part of kernel address, this way we can fit more address in same size of the buffer.
7. Create a shared memory between user and kernel to export the coverage.
8. We can use any combination of Linux kernel trace infrastructure is available like ftrace, ebpf, or modifying kcov or any other solution. Just make sure it is supported by the Android 12 kernel. 

User-space program design:
1. Coverage can be exported to different format for example lcov is one such format. But this might not be sufficient for export function graph-execution for that you will have to think hard about other format which can address this requirement.
2. The user-space program will be in the android device which will interact with the python program which is on the Linux machine which is connected to android device via adb.
3. The user-space program will collect the coverage and send it to the python program(external program). The python program will store the data in much efficient format like sqlite or json.

External Program Design:
1. The external program will capture all the driver which are loaded in the android device using 'lsmod' which has the base address stored, since this value changes on restart. It will also store the metadata like sha256 of the target drivers.
2. To figure-out basic block address external program can we can use capstone library. Function address can be obtained from 'kallsyms'.


### Acceptance Criteria
- Criterion 1

### Type
epic



## Android Kernel Coverage - ftrace approach

### Description

I am currently using kcov for syscall tracking which is doing a good job, but the area which it is tracing is very huge (core kernel and drivers) this leads to coverage buffer been over-ridden with coverage. In my opinion, the best way to solve this problem is to reduce the number for probes which are installed to collect coverage. But there might be other solution which you can suggest.

### Design

Target Environment:
- Target: Android kernel driver in ARM64 architecture.
- Access: Source code, kernel compilation config, root via adb shell.
- Full control over target system.

Driver Design:
1. use ftrace to address below requirement, also you can use ftrace function hook to trace function address and parameter.
2. It should be able to dynamically add and remove probe to the target driver.
3. The user should be able trace function multiple function. Ignore in-line function tracing.
4. There will be different types of probe of basic-block or function tracing
	1. single hit and remove probe - this probe remove once it executed
	2. max 'n' probe - this probe is remove once it is executed 'n' number of time.
	3. never probe - this probe always sticks around no matter how many time it has hit.
5. Tracing function call-graph is very important and unique feature of this driver. Do research on how this can be implemented.
6. The execution will place a filter on the process for which all this tracing should be done. This is very important or else we are tracing code which is not triggered by us.
7. Since the kernel address range is know, the address of the executed driver can be compressed by removing the common part of kernel address, this way we can fit more address in same size of the buffer.
8. Create a shared memory between user and kernel to export the coverage.
9. We can use any combination of Linux kernel trace infrastructure is available like ftrace, ebpf, or modifying kcov or any other solution. Just make sure it is supported by the Android 12 kernel.

User-space program design:
1. Coverage can be exported to different format for example lcov is one such format. But this might not be sufficient for export function graph-execution for that you will have to think hard about other format which can address this requirement.
2. The user-space program will be in the android device which will interact with the python program which is on the Linux machine which is connected to android device via adb.
3. The user-space program will collect the coverage and send it to the python program(external program). The python program will store the data in much efficient format like sqlite or json.

External Program Design:
1. The external program will capture all the driver which are loaded in the android device using 'lsmod' which has the base address stored, since this value changes on restart. It will also store the metadata like sha256 of the target drivers.
2. To figure-out basic block address external program can we can use capstone library. Function address can be obtained from 'kallsyms'.
