---
up: "[[Writing MoC]]"
tags:
  - "#type/os/linux"
status: todo
created-date: 2024-12-10
summary: Field notes about using Linux Kernel kcov
---



## Kernel Coverage Library

Collecting code coverage is an important task for many security assessment activities like fuzzing, code review and other dynamic analysis. You might want to know the code coverage of existing fuzz corpus or for an existing unit/regression test suite. This metric will help you to measure your progress, not only that but also reveal uncovered portion of code which are untested area of the project which are possibility hiding bugs. In this post I will describe challenges concerning Linux kernel code coverage and how can you go about implementing the solution, and at the end of the post there is link to the project where I have implemented everything which I described in this post.

### Collecting coverage for userland vs kernel

Collecting code coverage for user-space program is a bit easy, all you have to do is compile the program with code coverage flags in whatever compiler you are using, compiler will instrument the binary. This instrumentation code will collect and dump the code coverage on the disk. There will be some post processing tool as part of the compiler suite which will generate the HTML report. You do this is the case when you have the source code and you have the full control over compilation process. But, If you want to do this for black-box binary then there an entirely different tools and techniques you can use but still doable.

In case of kernel code the situation get a bit tricky, you need to compile the kernel with KCOV support to put the instrumentation code in the kernel along with that you need to do some API calls to actually collect the coverage. The reason for this additional call is that the kernel code is executed by many different processes and since the kernel is shared by whole system different user-space process are invoking the same code and its impractical run only processes you want to trace. So the kernel not just have to collect executed addresses but it also have to associate this coverage with a process. To make this possible to kernel KCOV module provides bunch of API's (system calls) which are expose to user-space process. You can integrate these system calls in the process which you are interested in tracing, these calls will notify kernel which process to trace. You need to do invoke these from every thread of the process not just from the main thread. Exact implementation details along with code example for kcov API's can be found from this [link](https://docs.kernel.org/dev-tools/kcov.html).

There is a very crucial difference between user-space and kernel space coverage which you need to understand. In case of user-space there is only one process accessing the code which we intend to trace, you don't have to worry about which other process executing the code. But for the case of kernel there are multiple processes executing the same piece of code and we don't want to collect coverage for other process other then the process we are interested to trace. 

### Hidden truth of kcov interface

One another important thing to note is kcov doesn't work in very complex settings what I mean to say is if, you call the kcov tracing API's at the entry point of the process and collect the coverage just before process exit's you won't get accurate coverage data and that's because that will be too much data for kcov buffer store and for some reason even if I provided large buffer I have not getting coverage of so part of drivers which I knew was executing for sure.

After lot of trial and error I realized the appropriate way to work with kcov interface is to enable tracing on the process and then reset the coverage before make **each system call** and once the call has returned immediately copy the coverage data in another buffer from the kcov buffer which you share with the kernel. I am surprised this is not mentioned anywhere in kernel documentation. And also this exactly what syzkaller does for its coverage collection on the fuzz cases.

### Practical issues when using kcov interface

In this section I will describe some practical aspect about using kcov in your project :
- For collection coverage you also have to allocate memory and give the pointer to the kernel where the kernel will store the coverage data. Make sure you allocate large enough space or you will be spilling some of the coverage. Tune this value by experimenting with the application you are tracing.
- If you work on kernel related project in Qualcomm you are usually dealing with proprietary code which is for the most case drivers code. To interact with the driver code you will have Userspace library which is making calls divers interface. For the most part you will interested in tracing the code which is executed by this driver. These library are complex and make system calls to many different driver interfaces using kcov in the manner we described previously impractical.
- There are couple of approaches you can take to work around the problem we described in the previous point and the fundamental ideas behind all these technique is that you hook all the system call you are interested in tracing and put the kcov call before and collect coverage after the execution. The system calls you mostly be interested will be the driver interface *ioctl*. The call to ioctl will be the most relevant part to trace because that's where all the custom commands will be executed. So of the methods you can use is
	1. LD_PRELOAD - in the method you library will be loaded by the OS loader before the all library and you can override the system-calls you are interested to hook.
	2. xHook Library - the above method will not override all the system methods in the process this issue can be fixed using [xHook library](https://github.com/iqiyi/xHook) will search all the method you want to patch in all library of the process and patch those symbols with the you custom function.
	3. ptrace - its a Linux API's for debugging applications, these API's are used by all the debugger to implement their functionality. You can use these API's to do system call hooking. It might be little complicated to implement but this is the most accurate method of all and you can remove and replace the hook as an when you need this might help if you in case when you want to extract little bit of performance in you application.
	4. frida - this is the DBI framework which supports ARM and all allow you to write hook for any user space application this is a bit sophisticated tool for our need but just another options.
- If your application multi-threaded which is very likely then you need to take that into consideration when writing your tracing tool, because you need to enable kcov on each thread and the coverage collection and storage has to be done on per-thread basis. It won't be good idea to have share data structure between process and implementing lock as it might disrupt the natural flow to the application you are tracing and might introduce unintended results.

To solving all the above mentioned challenges I have created [this project](https://github.qualcomm.com/mshelia/kernel_cov_collector) while i was working on fuzzing camera driver. You can use it in your project if applicable and provide feedback to me.