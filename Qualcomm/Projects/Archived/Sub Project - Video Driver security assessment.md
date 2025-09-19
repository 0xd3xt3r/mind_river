---
up: "[[WoS Kernel Driver Fuzzing]]"
tags:
  - "#qpsi/project"
  - "#type/sub-project"
  - "#type/os/windows"
aliases:
  - windows kernel fuzzing
  - WoS Kernel Driver Fuzzing
status: done
created-date: 2023-02-15
summary: Security Assessment of Windows Video Driver
---

## Executive Summary

- QPSI conducted security review into kernel driver for Windows-on-Snapdragon with the goal to provide assurance that our kernel drivers are secure, has no obvious defects and works as expected so that it can be confidently used by both customers and partners.
- We have managed to do code review and fuzzing for Multimedia like Audio, Video, Display and Graphics sub-systems. We prioritize these sub-system over others because they are the most target area by hackers and external researchers.
- In multi-media sub-systems we manage to find around 41 vulnerabilities(C: 1 - H: 12 - M: 27 - L: 1).
- There were also effort to fuzz other driver other that multimedia which yielded 35 vulnerabilities(H:18 - M:11 - L:4 - NA:2)

## Objective

[[Fuzzing]] the Windows Video driver

The high-level goal is to ensure that the **data** being sent from user space to kernel space via system calls or other mechanisms (offered by Windows or specifically architected by Qualcomm) is handled securely by the kernel driver developed by Qualcomm. There are three things to keep in mind

- It is **not** sufficient to send cherry-picked malformed data for the kernel driver to process; rather the requirement is to continuously generate the malformed data automatically via a coverage feedback mechanism so that data generations that uncover new paths be treated with more weight.
- The kernel must be compiled specially using the kernel address sanitizer as described [here](https://www.microsoft.com/en-us/security/blog/2023/01/26/introducing-kernel-sanitizers-on-microsoft-platforms/) to qualify as fuzzing.
- The code coverage achieved at the point of saturation (new block covered after more than 48 hrs.) is 70% and no crash is observed for every 1 million randomized data buffers that are passed at that point of saturation. I am using the 1 million metric from a very dated presentation from Microsoft.

My team has experience on the coverage feedback and malformed data generation and will provided whatever assistance is needed.

## Auditing Strategies

What we need from you is a test case that reads input from a file and sends data down to the various data handlers in the kernel. We can assist in creating the mechanism that randomly populates the file with the random input in a loop.

- **General application purpose** - What is the application supposed to do?
- **Assets and entry points** - How does data get into the system, and what value does the system have that an attacker might be interested in?
- **Components and modules** - What are the major divisions between the application's components and modules?
- **Inter-module relationships** - At a high level, how do different modules in the application communicate?
- **Fundamental security expectations** - What security expectations do legitimate users of this application have?
- **Major trust boundaries** - What are the major boundaries that enforce security expectations?

## PoC

1. Sreekanth Aila - Engineer, Staff
1. Srivatsan Veeraraghavan - Engineer, Principal/Manager
1. Sarath Angadala - Senior Engineer
1. Andrew Hamblin, test automation team
1. Mike Mahan

## Bug Reports

### CR Raised

| CR      | Description                                                                      | 
| ------- | -------------------------------------------------------------------------------- |
| 3506552 | Crypto Bugs                                                                      |
| 3506533 | Crypto Bugs                                                                      |
| 3498180 | ACPI Binary File parsing bugs                                                    |
| 3498165 | ACPI Binary File parsing bugs                                                    |
| ---     | ---                                                                              |
| 3527084 | OOR Bug while using the field parsed by ACPI Binary file                         |
| 3527113 | ArgumentGetBuffer - ACPI Binary file                                             |
| 3527130 | InitParserFromNestedPackage                                                      |
| 3527177 | OOR vulnerability in ArgumentMapStringToHANDLE and ArgumentMapStringToUINT32     |
| 3527388 | Integer overflow in ProcessEnginePackage function                                |
| 3527464 | Integer overflow leading to OOB in function ProcessEngineConfigProperties        |
| 3527570 | Integer overflow in function ProcessEngineSharedSmmu                             |
| 3527587 | Integer overflow leading to OOB write in function ProcessDisplayExtraSourceModes |
| 3527595 | Integer overflow leading to OOB write in function ProcessPageTablesPackage       |
| ---     | ---                                                                              |
| 3550376 | Video driver bug 1 - VedCmdBatch                                                 |
| 3550378 | Video driver bug 2 - VedCmdSetMarkerTimeStamp                                    |
| 3551729 | Integer overflow leading to OOB write in function LoadAndParseAcpiPowerMethod    |
| 3551744 | Pointer Arithmetic leading to OOB access in function ProcessAcpiComponentPackage |
| 3551749 | Integer overflow leading to OOB access in function ProcessAcpiComponentPackage   |
| 3551776 | Integer overflow leading to OOB write in function ProcessAcpiEnginePstateSetData |
| 3551792 | Pointer Arithmetic leading to OOB write in function ProcessAcpiCustomData        |
| 3631616 | Video Driver Escape Bugs                                                         |
| ------- | FastRPC Related                                                                  |
| 3644274 | FastRPC                                                                          |
| 3681400 | FastRPC                                                                          |
| 3557677 | FastRPC                                                                          |
| 3631580 | FastRPC                                                                          |


### Bug Notes

#### Binary file parsing in driver


The CR which I filed CR and is a valid security issue. The Tech team has agreed to fix it. But there are couple of other concerns which I discovered after having chat with the tech team.

I had discussion with the tech team https://orbit/CR/3498165 and https://orbit/CR/3498180 and they agreed to fix it within a week. But this issue is much bigger than what I have reported.

To summarize the issue :

- The kernel driver reads a registry key which has the path to a binary file which is later reads into kernel memory, the bugs is in parsing the binary file. The binary file is a ACPI configuration file which basically helps to configure the graphics hardware. Earlier they had this configuration as part of their code but later they decided to decouple the driver from it and create separate configuration file and thus we have this feature. This binary file is generated from INF configuration file there are two of these files. One by Qualcomm and one by the vendor.

There are two major concerns :
- The ACPI Configuration file which the Kernel Driver reads from File system as Stephen Well pointed out its only writable by root user and also the registry key is writable by root user. At this stage the graphics driver is factory installed in the device and there is no update provided to the customer but in future there will be a way to download newer version of the driver from Qualcomm's website and install it on your machine. When this feature will be made available this issue will have much higher rating because when I will be update a driver configuration file will be shipped with it, which the attacker can manipulate the it and then install the driver so this can be done by a non-root user. Right now its a medium rated issue where privilege user is attacking HLOS kernel. But once update mechanism is available it will become high rated issue. The above method of separating configuration file from driver is taken by other teams as well, so we also need to audit this approach in other Windows Driver like camera audio.
- ~~But there is much more pressing concern with this issue. The ACPI configuration ships certain parameters which decides the speed of the Hardware so basically a low tier hardware can work as high tier by changing values in this configuration file. This can happen even if entire file is valid with no malformed values. To perform this attack i could just copy the configuration of high tier hardware in low tier hardware and I won't even need to do complex reversing effort. This issue can cause direct monetary impact.~~ (This was not a valid issue, once confirmed by tech team)

To solve this problem we need to have signed configuration file. We can embed the configuration file in the driver file's resource section since the driver is signed resource section will also be signed and thus can't be tempered with. This issue is not easy to solve 

#### ACPI Configuration parameters

class ACPIParser reads binary file and maintains the pointer 

1. there is no max value check for \_strnicmp as *m_pActiveArgument->DataLength* can be controlled by attacker. Affected functions are (Reported the bug)
	1. ~~ACPIParser::ArgumentMapStringToHANDLE~~
	2. ~~ACPIParser::ArgumentMapStringToUINT32~~
	3. ~~ArgumentCompareString~~
	4. ~~ArgumentGetBuffer - this is very similar but instead uses **memcpy_s**
2. ~~LoadAndParseCRSMethod - 2 issues with argument parsing issue~~  (Not a reachable) Invalid issue
3. ~~LoadAndParseEngineMethod -> ProcessAllEnginePackages -> ProcessEnginePackage~~ (Reported the bug)
4. ~~LoadAndParseEngineMethod -> ProcessDisplayPackage -> ProcessEngineThermalDomains~~ - All ok no bugs in this path
5. ~~LoadAndParseEngineMethod -> ProcessDisplayPackage ->ProcessEngineOrDisplayResources -> ProcessEngineConfigProperties~~
6. ~~LoadAndParseEngineMethod -> ProcessDisplayPackage ->ProcessEngineOrDisplayResources -> ProcessEngineSharedSmmu~~
7. ~~LoadAndParseEngineMethod -> ProcessDisplaysPackage -> ProcessDisplayExtraSourceModes~~
8. ~~LoadAndParseEngineMethod -> ProcessPageTablesPackage~~
9. ~~LoadAndParseChildDevMethod (1 integer overflow issue) -> ProcessChildDevPackage (lots of isue with string length )~~ (Not a reachable) Invalid issue
10. ~~LoadAndParseAcpiPowerMethod(1 integer overflow issue) -> ProcessAcpiComponentPackage (2 issues)~~
11. ~~LoadAndParseAcpiPowerMethod -> ProcessAcpiComponentPackage -> ProcessAcpiPstateSet -> ProcessAcpiEnginePstateSetData(6 issues) -> ProcessAcpiCustomData~
12. LoadAndParseAcpiPowerMethod -> ProcessAcpiComponentPackage -> ProcessAcpiPstateSet -> ProcessAcpiEnginePstateSetData(6 issues) -> ProcessAcpiPstate -> ProcessAcpiCustomData
13. BuildConfigTreeFromACPI

## Notes

- Search for keyword "windows device" in web streams.
	- There aren't much resources on this in open webstream and even which are there they are very old.
- What is your test setup?
	- Makena we will use this currently
	- MDAG will be ported to Hamova 1.1
- What is DDI's ?
	- This is interface between windows driver and windows core kernel service
- MITM from Microsoft
- [Integrating ARM CoreSight with AFL ](https://ricercasecurity.blogspot.com/2021/11/armored-coresight-towards-efficient.html)
- Windows Drivers Entrypoint - `NTSTATUS DriverEntry(...)`  
	In our case, this function is responsible for:
	1.  Creating a device object that will receive control commands.
	2.  Registering callback methods that Windows calls for the created device objects. For example, in the case of a request to read data from the disk, the system will call a read callback in our driver.
	3.  Registering the **DriverUnload** method, which Windows calls to unload the driver and free the allocated resources.
- Kernel Attack Surface
	1. Device - IOCTL call
	2. Kernel Object - shared with the user, so user can manipulate it eg timer
	3. Major Functions - used by VFS drivers also be used by other drivers
		1. attack driver with syscall NTWriteFile NTReadFile this can also be used for device objects as well
	4. OS Using certain features which then uses our drivers ed Dxgkddi
- How to collect logs of driver execution
	- Get All_Log_Suite Tools
	- Get Build Id of the target from registry from *HKEY_LOCAL/System/Software/Qualcomm*
	- in all log collection select the type of logs you want to collect this program will generate script 
	- run the script in the above step to start the log generation process
	- the run use case
	- running the same script will stop to log generation step. 
- General Media Concepts [link](https://learn.microsoft.com/en-us/windows/win32/medfound/media-foundation-programming--essential-concepts)
- 

## Challenges

- Collecting [[Code Coverage]]
	- Coverage feedback we used Bullseye
		- Bullseye is that it can instrument Windows kernel drivers
		- But it's too slow, as it relies on an external executable to parse a file on the disk
		- If I were to do such a project again, I'd try writing a custom runtime for Bullseye to use shared memory.
		- Would use ETM for collecting coverage
			
	- ARM [[CoreSight]] [[ETM]] for code coverage
		- We tried using ETM, we found a way to dump the ETM buffer, but didn't go as far as writing a parser. (Alex)
			- There's nowadays a fork of AFL on the internet for parsing ETM (CoreSight) for Linux, I think we could adapt it to Windows.
			- *WP\QDSS\rel\10.5\qdssTestApp\QdssTestApp.cpp* - This is the app which can dump ETM on Windows
			- Though for a start I think you should just check if a dumb fuzzer works if you provide it with the dictionary (grab IOCTL codes from the disasm/C headers)
		- Existing Project
			- https://github.com/AFLplusplus/AFLplusplus/tree/stable/coresight_mode
			- https://github.com/RICSecLab/coresight-trace
- Coverage Feedback Fuzzer
	- https://github.qualcomm.com/guilli/afl_windows/

## Attach Surface

- Reference Research [link](https://www.blackhat.com/docs/us-14/materials/us-14-vanSprundel-Windows-Kernel-Graphics-Driver-Attack-Surface.pdf)


## Internal Code Resources

- Pls go over the New Hire docs in - \\HARV-SVEERARA\dropbox\VideoNewHire
- [Grok](http://wmlab-224:8080/source/xref/SC8280X.WP_MK.1.0/direct3d_kmd/rel/10.6/kmd/driver/vidLib/)
- [DX11 UMD](http://wmlab-224:8080/source/xref/SC8280X.WP_MK.1.0/direct3dum11/rel/10.6/direct3d/dx11/video/)
- [DX12 UMD](http://wmlab-224:8080/source/xref/SC8280X.WP_MK.1.0/direct3dum11/rel/10.6/direct3d/dx11/video/)
- Perforce Unittest code - //source/qcom/qct/multimedia2/EmbeddedA/Video/Drivers/Codec/Venus1.0/test/main/latest/UnitTest2/
- Perforce Unittest solution file to open from visual studio - //source/qcom/qct/multimedia2/EmbeddedA/Video/Drivers/Codec/Venus1.0/test/main/latest/UnitTest2/platform/common/ved_test/ved_test_main_wp63.sln
- KMD Location - //source/qcom/qct/multimedia2/graphics/wpci/GraphicsMain/rel/direct3d_kmd/rel/10.6

## Learning Resources

- Resource to learn about Windows DirectX
	- [What is graphics Pipeline](https://www.youtube.com/watch?v=gQiD2Kd6xoE&list=PLplnkTzzqsZS3R5DjmCQsqupu43oS9CFN&index=2&ab_channel=CemYuksel)
	- [Learning OpenGL](https://www.youtube.com/playlist?list=PLvv0ScY6vfd9zlZkIIqGDeG5TUWswkMox)
	- [C++ 3D DirectX 11 Tutorial](https://www.youtube.com/watch?v=2NOgrpXks9A&list=PLqCJpWy5Fohd3S7ICFXwUomYW0Wv67pDD&index=2&ab_channel=ChiliTomatoNoodle)
- [Micrsoft Documentation](https://learn.microsoft.com/en-us/windows-hardware/drivers/display/mapping-virtual-addresses-to-a-memory-segment?source=recommendations) on Mapping virtual memory to video memory

## Collecting Code coverage
- Code coverage collection can be done with ARM CoreSight [example link](https://github.com/ARM-software/CSAL/)
- Linaro OpenCSD - CoreSight Trace Decode [link](https://github.com/Linaro/OpenCSD)
	- Blogpost https://www.linaro.org/blog/opencsd-operation-use-library/

## Presentation Content

1. Testing very complex scenario
2. Protobuf based grammar
3. completed full loop
4. harnessing architecture overview
5. support sequence of API calls to test complex state machine

## Timeline
- 15-02-2023
	- Discussion on fuzzing started from management
	- Start the research on my part (not really)
	- Had call with the team
- 28-02-2023
	- got access to code and Visual studio IDE environment
- 01-03-2023
	- had chat with the window's team they want to take over the fuzzing effort the minimize the effort required.

## Ref
1. [Getting Started with Windows Mobile Drivers (WM201A)](https://web.microsoftstream.com/video/215b8b41-1baf-4f89-a32d-1619d267defb)
2. [Demo: Using windows device and Windbg.](https://web.microsoftstream.com/video/43aa34fc-a4b2-42c3-a626-169bb73e628f)
3. https://web.microsoftstream.com/video/77ef897b-3dd6-469d-8375-a5e8bfb9e774
4. http://www.alex-ionescu.com/
5. [Nvida Video encoding using D3D11 API's](https://github.com/NVIDIA/video-sdk-samples/blob/master/Samples/AppEncode/AppEncD3D11/AppEncD3D11.cpp)
6. https://confluence.qualcomm.com/confluence/display/videoDebugging/Video+Debugging
7. https://www.zerodayinitiative.com/blog/2018/12/4/directx-to-the-kernel
8. [Sample Video Exploit](https://github.com/pancudaniel7/exploitdb/blob/0e3474b2ca643034349a54a650ea5361e7c59cc2/exploits/windows/dos/47797.c)
9. https://github.com/sailfishos/virtualbox/blob/master/virtualbox/src/VBox/Additions/WINNT/Graphics/Video/mp/wddm/VBoxMPWddm.cpp#L3399

[[Sub Project - Fuzzing Windows Driver IOCTL Interface]]