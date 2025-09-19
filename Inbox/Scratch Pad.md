# Scratch Pad

Not down random stuff which you don't know where to put it.

# Camera Driver Syzkaller fuzzing 

## Tech team PoC Help

I went through the camera documentation and things are very well described on architectural level. I think it will help us to understand how to fuzz Camera system effectively. Going the through the camera subsystem I also realized that there are certain use-cases which cannot be directly expressed in syzkaller grammar.

The reason for the above claim is that, in the camera sub-system communication between UMD and KMD is done by CSL Layer which is consumer/producer design pattern where the command which are to be execute are queue and kernel driver then picks up the command and execute it. The pattern of pushing and executing of command is done via two different call which will be very tricky for the syzkaller to model in this case. We need use hybrid approach of defining packet grammar in syzkaller and write pseudo syscall in syzkaller to queue that message and I think is is doable if tech team and security team collaborates.

The reason for writing this email is I need your help to write these pseudo syscall for sensor and actuator sub-systems(for the start). I am aware that there is Camera UMD API's which can help use to achieve this, and I already did try to do it on my own but I failed to setup the development environment and also I couldn't find camera API documentation. If someone form tech team can help us with this it will help use to increase the code coverage.

The main idea I want to push forward here is the that is camera is extremely stateful application and to get coverage of certain part of program we need to get the driver in particular state to get execution. There many such feature which we can test, and these test document if form code in the test suite which camera tech team has written and currently it has very good coverage. If we manage to translate these test case to syzkaller fuzz suite we will have very great code coverage. But, we have to be mindful when using this approach because I don't want to replicated the functional test in to fuzzy testing what I intend to do is identify the data which the user controls and create syzkaller grammar then wire-up the syzkaller grammar with the camera test cases. For the start we will target sensor and actuator sub-system.

There is another test suite which is at URL **git clone -b mainline ssh:\/\/$USER\@review-android.quicinc.com:29418/platform/hardware/qcom/camx** I need help for compiling.

## part 2

Is there something the camera CSL unit test need to do for camera Syzkaller?
Syzkaller seems like syscall level unit test which is the below CSL.
Do we need additional camera unit test framework which calls syscall directly without CSL?


Yes are you are correct syzkaller uses system level calls to do it fuzzy testing. 

My interest to explore unit-test for syzkaller come from the point of view that I want to repurpose the existing unit-test cases for fuzzy testing. There are certain test cases which are executing/testing certain scenarios and what I want to do is corrupted data which user can control. Another the reason to use existing test is that different device need different type of initialization procedures and doing all those thing again form scratch will be very time consuming.

I have recognized that packet architecture is how the communication between the UMD and KMD is happening and I want to write fuzzy test for all the packet which are send from the UMD to KMD. Currently I am able to achieve this (with my naive understanding of the code) for CSIPHY device I want to extend this to all other devices.

CSL Layer API's is the closed layer to system call level so I don't think we will be needing direct system-call level code CSL seems to be sufficient. The only challenge I see is how can we compile the CSL module along with unit test code such that we can integrate it with syzkaller.

What help do I need?
1. Can you help me to compile CSL module as library, currently is it is used by all other module my importing the code by the CSL code and it is not compiled as share library something like libcslcore.so. If I can get this it will be used to compile unit test case code into syzkaller.
2. I can see that in cslunittest folder there are no test cases for actuator device if will be of great help if you can add test cased for it.


## ARM 64 Tracing Library

1. Jtrace is exactly what we need to create and its closed source and not for commercial use. It is a derivative of strace library.

### Ref
1. https://github.com/gaffe23/linux-inject
2. http://newandroidbook.com/tools/jtrace.htm
3. https://github.com/uber-archive/pyflame/blob/master/src/ptrace.ccl

## Hypervisor Fuzzer

Selecting fuzzer which has following criteria
1. Ability to express grammar
2. Code Coverage feedback
3. Crash detection
4. Action
	- First, print arguments that drivers using Gunyah provide to HVC calls, particularly capabilities
	- Then, without fuzzing, try reproducing with the same values
	- Add a simple kernel driver to expose Gunyah interface via IOCTL/debugfs and create Syzkaller grammar
	- Examine coverage via ETM
	- If that succeeds, modify Syzkaller to support ETM coverage for HV

## Fuzzing Project

- Targeting windows
	- https://www.mandiant.com/resources/blog/fuzzing-image-parsing-in-windows-uninitialized-memory
	- https://learn.microsoft.com/en-us/windows/whats-new/whats-new-windows-11-version-22h2
		- High Efficiency Video Coding (HEVC) support
- Oracle Outside In Technology
	- Outside In Technology is a suite of software development kits (SDKs) that provides developers with a comprehensive solution to extract, normalize, scrub, convert and view the contents of 600 unstructured file formats.
	- https://www.oracle.com/middleware/technologies/outside-in-technology-downloads.html#oitv857-tab

## Process Parsing Prompt

I want you to parse proc file system in linux and I want you to create relation between parent process and the threads in different structure. And I want you to write the code in cpp
also parse the memory map of the process
I want you to parse only for one process and the memory region permission should be as enum. The thread info structure should store reference to process struct

I want you to write a proc file system parser process for linux in cpp which has additional following requirement
1. parse the memory map of the process and the memory region permission should be enum.
2. Parse thread information and create the relation between the thread and the process.
3. Also parse all the open file descriptor for shared memory, files and network socket
4. use c++11 standards and please don't create nested classes.
5. The class should have away to only parse for one process which you will take as a command-line argument
6. this doesn't have detail socket parsing, can you include parsing of detail information from /proc/net