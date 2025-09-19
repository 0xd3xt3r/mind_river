# Resume

## Qualcomm

- Worked with the engineering team to verify the patch and propagate it across different branches.
- Conducted source code auditing in collaboration with engineering team to understand and create understand of the attack surface. This is also later helpful for fuzzing.
- Proactively look for bugs in Qualcomm code base by analysing the bug reported to Qualcomm by external researchers, based on bug reports develop Klockwork rules to identify the bugs.
- Training developers on software security practices and conduct test to assist develop to use klockwork static analysis tools. 

### Project 

1. Fuzzing Telemetry and Coverage reporting
	1. Qualcomm had tons of C/C++ project which has protocol parser, file format, and audio/video decoders as part of its code base. Fuzzing  harness are written to fuzz with AFL but these projects are embedded as part of many project which very large code based and complicated build process, and these harness are spread across different code bases. There was no way of bringing all the project under one hood and fuzzing effort distributed across different team. Especially since different projects have different build systems and project owners. We also wanted to track the code coverage of the all the projects and generate and HTML source coverage report. 
	2. To solve problem #1, I patched the AFL project to send the fuzzing telemetry to the central server. The server collected all statistics for various fuzzing project which were actively running this helped us to track all the project with fuzzing metrics of each project been display on single dashboard.
	4. There was an existing tool which collected basic block coverage using Intel Pintool, I was very slow took about 2 days to collect coverage for 3000 seed corpus for a single fuzzing project. We had hundreds of such projects fuzzing project. And it report only the basic block coverage.
	5. I refactored the old Intel Pintool program to improve the coverage collect speed. Then covert the basic block coverage to function and line coverage report using addr2line and stored the result in lcov coverage format. Then we can use lcov suite of tool to do other post process but importantly get an HTML coverage report. The HTML report has source code highlighting covered and uncover function and lines. All this thing was glued together with a python tool which executing the coverage collection, consolidation and processing. 
	6. All above steps was scaled on using python multiprocessor module to achieve faster speed and one we could generate coverage report for 3000 seed in 3 hours. This tool was used by security and engineering team to improve fuzzing harness and the metric was later used in software releases and monthly security updates.
1. Windows Device Driver Fuzzing Framework
	1. Windows-on-Snapdragon Platform is a Windows Laptop on ARM SoC has hundreds of Kernel driver, these kernel driver are and attack surface which needs to be secured so in order to ensure the security of the Qualcomm Laptops various security activities were carried out like code auditing, static analysis and fuzzing.
	2. Due to lack of open source tools for Windows Kernel Fuzzing so I decided to build the tool  from scratch. The Design Goal of the fuzzer was to have a continues fuzzing of all the Kernel Driver which had features like crash triage, bug reproducing and corpus minimization and fuzzing on device farms.
	3. The fuzzer is a client server architecture and the fuzzing client is inside the windows target and the Master which is running on different machine which is connected with the fuzzing client via socket. The central server monitored the client for crashes using windbg, and coverage was gathered using Bullseye and integrate coverage feedback in the fuzzer.
	6. Fuzzing client was designed to do Structure Aware and Stateful fuzzing. Implemented a fuzzing engine library which exposed API to describe fuzzing API along with the function prototype. The prototype definition was then use to do structure fuzzing. The prototype described to structure of input and special mutator functions were written to mutate them, they were part of the library. This can be used to describe both primary and composite datatype like struct, nested struct and array. The fuzzing engine was also designed to uncover race-condition bugs.
	7. A very basic Web Dashboard was expose by central server to show the overall fuzzing metrics.
2. Improve Syzkaller Fuzzing
	1. The goal of this project was to improve existing Syzkaller harness for android kernel drivers to improve the coverage since it was out dated for quiet a while. Qualcomm has hundreds of kernel driver which it needs in order to support is SoC. So the effort to increase the coverage required mixture of manual and automated approach. 
	2. Driver are difficult to fuzz because it has complicated initialisation process which depends on SoC and hardware and it is also very stateful this leads to areas which are protected by state verification.
	3. I started with Manually review the kernel driver to updated structures and ioctl calls which were missing and update the Syzkaller grammar. Also figured out different ways the user-space library was shared the data with kernel like shared buffered, etc.
	4. I also came up with a technique that utilise the exist unit test case for the kernel drivers to capture the parameter which will give deeper coverage. Also by running the Android OS applications which will triggers the functionality which pertain to the kernel driver and capture the parameter to the interaction. The capturing of the parameter was done using Frida DBI framework. The captured data was convert it in to sykaller corpus, this ensured that the syzkaller will atleast get the baseline coverage as that of unit-test cases.
	6. Some of the driver needed complicated state setup and to do this I had to add pseudo syscall which is a small C code which does the state initialisation and forwards the data appropriately. 

## Payatu Work Profile

- In-depth work with security threats, vulnerability along with practising security development life-cycle practices.  
- Threat modeling the client products
- Security code review, analysis and vulnerability assessment.  
- Analyzed findings and generating reports.  
- Reverse engineering binaries for Windows, Linux OS (and at all layers Boot, Kernel space, User space, ) and Bare-Metal 
- Security testing techniques including fuzzing and pen-testing on a both API, Drivers, Libraries & Executable.  
- Using OS debuggers such as x64Dbg, OllyDBG, WinDBG, GDB/PwnDBG, and Immunity Debugger custom build PyCommands.  
- Using reverse engineering tools such as Radare2, IDA Pro, Ghidra.  
- Using pen-testing methods such as Fuzzing, Remote Code Execution, Code Injections, Shared Library Injection/Hijacking, DLL Load-Order Hijacking, ROP/Stack/Heap/UAF Buffer Overflows, Shellcode Exploitation, Format-String Attacks, Symlink Attacks, & LFI/RFI.  
- Use of Applied Cryptography when dynamically analyzing obfuscated/encrypted communication.
- Create security static & dynamic tools (Python & C/C++ Programming) for source code auditing for clients  
- Applied security practices for embedded systems on CMSIS-RTOS/FreeRTOS, drivers, and along with firmware security testing methodologies on various MCU's.