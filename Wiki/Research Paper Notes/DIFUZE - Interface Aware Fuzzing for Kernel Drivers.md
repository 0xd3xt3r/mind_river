---
up: "[[Research Paper MoC]]"
tags:
  - "#reading/research-paper"
status: todo
created-date: 2024-12-16
---

> **summary**:: Paper on Linux Kernel Fuzzing

### Problem 
- Too much manual work to write syzcall description for syscalls
- Vendor drivers are biggest attack surface

### Solutions
- Use static analysis method on LLVM Bitcode to recover interface structure which are used by the ioctl interface
- using list of kernel structure to identify entry pointer

### unsolved problem/future work

- using list of kernel structure to identify entry pointer is not comprehensive approach.

### Notes

- Generic fuzzers like AFL will not have Live Pointers(struct data containing active pointers to other data).
- List of structs from which you can find IOCTL handlers
- There are other dynamic analysis technique where you simply attach to the running process you hook to ioctl function. Once you have this control you mutate the value when the call is made.
	- Limitation of this method is that you are not possibly exploring all the code-paths and are only limited to the calls made by the testing application.
	- You don't have the type information of the call argument, this doesn't allow you to make intelligent mutations.
- *Interface Recovery*
	- cross-dependency between *command id* and corresponding command pointer.
	- *Build system instrumentation* to identify the build command for gcc and re-run those commands for LLVM compilers.
	- Run inter-procedural path-sensitive analysis to collect all the *Equality Constraints* on the command value.
		- *Range Analysis* to recover possible value of *command id*. You need this analysis when the commands are nested for example in v4l otherwise Equality constraints is enough.
- IOCTL Handler Identification is a important problem as this will allow you to identify the attack surface.
	- Linux Kernel has many ways of doing this, one of the main technique.
- IOCTL *command id* and command struct has **many-to-many relationship**. Single *command id* can take several struct and single command struct type can be used by multiple *command id*.
- Find all the path from *ioctl handle* to *copy_from_user*, and the source operand of the *copy_from_user* should be from *command pointer* from *ioctl handle* function. Then we try to find the *type* of source operand.
	- while going to different code path you type of the source can under-go multiple type casting which could generate false-positive for this analysis.
	- To handle the above problem do type analysis on the source type of the *copy_to_user* function.
	- you can also consider the original destination parameter of the *copy_from_user* function and the copy size.
- To associate the *command type* to *command id* we collect equality constraint along while performing type propagation. The constraints on the command value on path reach *copy_from_user* represent possible *cmd id* and *cmd type*.
- Finding struct definition can also be very tricky because you not only have to find top level structure but only also the nested structure along with type of the pointers.
- IOCTL registration structure - there are different ways to register file structure to the core kernel and those registration structure has ioctl file handler function pointer.
	- what writing LLVM pass you can find the store instruction on the one of the fields of any operations structures like `video_device`, `v4l2_file_operations`, `file_operations`, `v4l2_subdev_core_ops`, etc.
- Statistical question after running above analysis.
	- how many command identifier in each *ioctl handler*
	- how many of them have copy_from_user and copy_to_user.
		- some of the time you don't need this since the 
	- what are the argument types?
		- how many arguments are primitive data type
		- how many are simple struct
		- how many are structs with embedded pointer.