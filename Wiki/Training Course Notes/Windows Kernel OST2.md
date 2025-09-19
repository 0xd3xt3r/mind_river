---
up: "[[Learning MoC]]"
tags:
  - "#computer-architecture"
  - "#learning/course"
  - "#training/ost2"
title: Intermediate Windbg
status: done
summary: Windows Kernel Driver related notes
---

# Intermediate Windbg
## Commands

- In WinDbg, hit break, and then enter the command `ed nt!Kd_IHVDRIVER_Mask 0xf`. 
	1. This is what makes debug prints from kernel drivers show up in the debugger.
- The **B**reak **U**ndefined ([bu](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/bp--bu--bm--set-breakpoint-)) command has the same syntax as the bp command, so you can issue the following command to break on the as-of-yet-undefined location of KmdfHelloWorld's DriverEntry() function.
	1. `bu KmdfHelloWorld!DriverEntry`
-  Displaying Local Variables
	- When you are source-level debugging, you can use the "**D**isplay Local **V**ariables" ([dv](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/dv--display-local-variables-)) command, or you can open the Local Variables window as shown in the below video. Note however, that if you have not yet stepped through the function prolog, and into the function body, the debugger will display incorrect values for variables.
		```
		Breakpoint 0 hit
		KmdfHelloWorld!DriverEntry:
		fffff804`600b1000 4889542410      mov     qword ptr [rsp+10h],rdx
		1: kd> dv
		   DriverObject = 0xfffff804`5875cecc Driver {...}
		   RegistryPath = 0xffffb38b`269ff000 "\REGISTRY\MACHINE\SYSTEM\ControlSet001\Services\KmdfHelloWorld"
				 config = struct _WDF_DRIVER_CONFIG
				 status = 0n5
		1: kd> t
		KmdfHelloWorld!DriverEntry+0xe:
		fffff804`600b100e c744243000000000 mov     dword ptr [rsp+30h],0
		1: kd> dv
		   DriverObject = 0xffffb38b`26375e30 Driver "\Driver\KmdfHelloWorld"
		   RegistryPath = 0xffffb38b`269ff000 "\REGISTRY\MACHINE\SYSTEM\ControlSet001\Services\KmdfHelloWorld"
				 config = struct _WDF_DRIVER_CONFIG
				 status = 0n535508544
		```
- Displaying Structures
	- The "Display Type" (dt) command can both display the fields of a structure, and interpret the memory at a given address according to a given structure type.

	- Display Structure Definition
	- To only display the top level fields of a structure of a given type:
		```
		dt {structure type}
		
		0: kd> dt nt!_EPROCESS
		   +0x000 Pcb              : _KPROCESS
		   +0x438 ProcessLock      : _EX_PUSH_LOCK
		   +0x440 UniqueProcessId  : Ptr64 Void
		   +0x448 ActiveProcessLinks : _LIST_ENTRY
		   +0x458 RundownProtect   : _EX_RUNDOWN_REF
		...
		```
- Recursively Display Structure Definitions
	- To recursively display the fields of a structure (and sub-structures):
		```
		dt -r{depth} {structure type}
		
		0: kd> dt -r2 nt!_EPROCESS
		   +0x000 Pcb              : _KPROCESS
		      +0x000 Header           : _DISPATCHER_HEADER
		         +0x000 Lock             : Int4B
		         +0x000 LockNV           : Int4B
		         +0x000 Type             : UChar
		...
		```

 - Display Memory as Structure
	- To interpret the memory at a given address according to a structure definition:
		```
		dt {structure type} {address}
		
		0: kd> dt nt!_EPROCESS ffffb38b26516340
		   +0x000 Pcb              : _KPROCESS
		   +0x438 ProcessLock      : _EX_PUSH_LOCK
		   +0x440 UniqueProcessId  : 0x00000000`00001858 Void
		   +0x448 ActiveProcessLinks : _LIST_ENTRY [ 0xffffb38b`25aab748 - 0xffffb38b`263da4c8 ]
		   +0x458 RundownProtect   : _EX_RUNDOWN_REF
		   +0x460 Flags2           : 0x200d080
		...
		```
- The **S**et E**x**ception (sx) [family of commands](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/sx--sxd--sxe--sxi--sxn--sxr--sx---set-exceptions-), can be used to break when particular events occur. 
	- The most important forms are:
		- **E**nable: sx**e**
		- **I**gnore: sx**i**
	- There is a [long list](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/controlling-exceptions-and-events) of possible exception events, but for now we want to just look at the module **l**oa**d** (ld) event.
	- Break on module loads `sxe ld`
		- The debugger will break on every module load. You will be stopped before any of the code from the driver has executed, thus allowing you to set breakpoints, tweak memory, etc.
	- Automatically run command on sx* breaks
		- sx* commands can also take an optional _-c "some ; list ; of ; commands"_ parameter.  
		- A useful command to use is ".lastevent", as this will print out information about the last event that occurred. So for instance. `sxe -c ".lastevent" ld`
			-  Notification-only  If you'd just like to see when modules are loaded, but not actually stop (e.g. to just watch what order kernel drivers are loaded at boot time), you can use: `sxi -c ".lastevent ; g" ld`
	- Ignore/Disable breaks - If you used the simple form of "sxe ld", it can be disabled via the "sxi ld" command. `sxi -c "" ld`
- List Modules
	- List kernel modules only `lm k`
	- `lm a address` - If you'd like to see if a given memory address resides within the address range of a particular module, this can be done with
	- List Modules Verbose Output `lm ksmv`
- Changing process contexts with !process and .process
	- Display the current process context - `!process -1 [flags]` `!process -1 0`
	- Search for the process context based on executable name: `!process 0 [flags] [exe name]`  `!process 0 0 notepad.exe`
	- Search for the process context based on executable process ID (PID): `!process [PID] [flags]`
		- Where the Process ID can be found with tools like Task Manager.  `kd> !process 0x1b44 0`
	- Change process context to target executable: `.process /i /r /p ["PROCESS" address from !process output]`
		- `0: kd> .process /i /r /p ffffcd089d5d4300`
	- Example output with higher verbosity flags `0: kd> !process 0 f cmd.exe`
- Using `!pool [addr]` to associate a memory address with a data structure or driver
	- The Windows kernel has multiple "pools", which are really just heaps from which memory can be allocated. Pools can have different "tags", to help developers identify which allocations came from their own code (if they specified a custom tag at allocation time), or when the data is from a pool of all the same type of data structures.
	- Sometimes when analyzing Windows itself, the `!pool` command can be useful to sanity check if a random memory address happens to be associated with a given pool type which is used exclusively for a given structure type.
		```
		0: kd> !process -1 0
		PROCESS ffffdc0155e94340
		    SessionId: 0  Cid: 0d94    Peb: 1585fea000  ParentCid: 029c
		    DirBase: 125bbe000  ObjectTable: ffff81011ceac140  HandleCount: 1132.
		    Image: MsMpEng.exe
		
		0: kd> !pool ffffdc0155e94340
		Pool page ffffdc0155e94340 region is Nonpaged pool
		 ffffdc0155e94000 size:  250 previous size:    0  (Allocated)  MmCi
		 ffffdc0155e94250 size:   60 previous size:    0  (Free)       7+.#
		*ffffdc0155e942c0 size:  d00 previous size:    0  (Allocated) *Proc
		        Pooltag Proc : Process objects, Binary : nt!ps
		 ffffdc0155e94fc0 size:   20 previous size:    0  (Free)       .+.#
		```
		
	- A list of example pool tags is [here](https://github.com/jjzhang166/windbgtool/blob/master/Dependecies/x64/triage/pooltag.txt).



# Debuggers 3011: Advanced WinDbg


- This training help to setup a Windows machine for kernel debugging with virtual machine to another virtual machine, where one machine has all the software installed for reversing and debugging and other machine is the debugging target.
- Some of the tools which it uses are Windbg and other disassembly tools like IDA, Ghidra.  You can syncronize between debugger and Disassembler using [ret-sync](https://github.com/bootleg/ret-sync) plugin.

## Connection Method

There are three method of connection
1. VirtualKD - [VirtualKD-Redux](https://github.com/4d61726b/VirtualKD-Redux) is a software which you need to install in the target device. It is the fastest method of all three
2. Network
	1. Shared secret key between both VM
	2. You need IP address and secet key to connect
	3. 
3. Serial - 
	1. Add serial port in both machine. 
	2. You have to set the baudrate

## Misc

1. Set environment variable for symbol path so that you don't have to do it on every lunch
	```
	_NT_SYMBOL_PATH=SRV*c:\symbols*http://msdl.microsoft.com/download/symbols
	```
1. 