---
up: "[[Qualcomm MoC]]"
tags:
  - "#qpsi/report"
status: in-progress
created-date: 2024-12-13
summary: Annual Review Writeup
---

This file has all the contributions I made to different projects so that it helps me to write annual review.

## 2025

### Random Logs
1. Bharath Gandhi - Klockwork wifi issue

### Annual Goals

1. Fuzzing Farm for Syzkaller
2. On Target Hypervisor Firmware Fuzzing
3. Fuzzing Qualcomm Android Driver with Syzkaller
4. FastRPC Training in Hyderabad Location

### Review Write-Up

### Analytical Skills

### Communication

### Creating the New & Different

### Building Trusting Relationships

### Decision Making

### Getting Work Done

### Mentoring and Coaching

### Technical Knowledge
- Syzkaller internals to improve our fuzzing efforts

## 2024

**Position** : Senior Lead Engineer

### Goals

1. Linux Driver to do Research on Qualcomm's Devices
2. Hypervisor Fuzzing
3. Linux Kernel Fuzzing with Syzkaller
4. Fuzzing Windows Kernel Driver
5. Improve understanding of Qemu emulator so that we can load different Qualcomm Firmware and fuzz it.

### Review Write-Up

### Analytical Skills

Proficiency Rating: 4

#### Automotive Graphics Driver Code Review

After reviewing an incident report for HGSL driver, out to curiosity I started review the code base and found 2 high severity bugs and filed corresponding CR’s.

#### WoS FastRPC Driver Code Review

Noticed an incident reported for Windows FastRPC Driver. I started investigating the incident and based on this analysis I found the variable controlled by the attacker and by doing a bit of code analysis I found 3 bugs for which corresponding CR’s were reported.

#### WoS Device IOCTL Interface Fuzzing

The objective of this fuzzing initiative was to employ a Blackbox fuzzing technique to evaluate the IOCTL interface of Qualcomm’s Windows drivers. A specialized algorithm was developed to scan and fuzz the IOCTL commands.

The fuzzer begins by scanning all Qualcomm drivers installed on the machine. Each driver is then examined for IOCTL codes, which are saved in a database. These IOCTL commands are used to estimate the buffer size required by each command. With this information, the fuzzing process for the driver commences.

During this exercise, 78 drivers were identified that exposed the IOCTL interface. A total of 314 IOCTL command codes were discovered. The fuzzer successfully identified 22 crashes, and corresponding change requests (CRs) were reported.

#### WoS WiFi Driver Stateful Fuzzing

This fuzzer was and extension of WoS IOCTL fuzzer, but this fuzzer took a Whitebox approach and by reviewing the code I was able to integrate driver specific grammar which further improved the code coverage which resulted in 13 crashes for which corresponding CR’s were reported.

#### Android Kernel Fuzzing with Syzkaller

We are enhancing the security of the Android Kernel Driver by improving the Syzkaller descriptions. The existing code coverage is suboptimal because the syz-descriptions haven’t been updated in a while. I began focusing on the camera driver due to its high number of incident reports. Through thorough research, I discovered that the primary reason for low camera coverage was its unique user-kernel communication method. I successfully integrated this method into Syzkaller, improving the descriptions and unlocking many new code coverages, thereby enhancing the driver’s state.

The camera driver has 136 IOCTL commands. I added syz-descriptions for the TPG, Actuator, EEPROM, Flash, and OIS subsystems, covering 24 IOCTL commands. This work has also been merged with the Linux Security Team’s upstream repository.

### Communication

Proficiency Rating: 3

I have a strong ability to explain complex information in a simple and easy-to-understand manner. This is evident from the emails I wrote during the WoS Driver Fuzzing project, where I discussed fuzzing results, timelines, and plans to address issues. I regularly sent emails summarizing the project’s status and obstacles. Additionally, I presented this work at the QPSI monthly meeting, which received positive feedback.

In projects like the Android Syzkaller Project and WoS IOCTL Fuzzing, I not only released the source code within Qualcomm but also documented how to use the project and the design decisions I made to overcome engineering challenges. This approach has two main benefits: it allows others to use these tools with minimal support from me, freeing up my time for other tasks, and it provides detailed documentation of design decisions, enabling others to understand the tool’s internals and modify it to fit their use cases or improve existing functionalities.

Project which I am talking about:

1. https://github.qualcomm.com/mshelia/generic_ioctl_fuzzer
2. https://github.qualcomm.com/mshelia/syz-fuzz-v2

### Creating the New & Different

Proficiency Rating: 4

#### Fuzzing Qualcomm Android Drivers with Syzkaller

Fuzzing stateful driver is very complex since you need to have lot of knowledge about the target to get good coverage. But in Qualcomm very have access to source of project fully and access to unit test code. If we can extract out the call sequence and parameter value from the code then we can reach all those states which the unit-test has access to, this can be automated to certain degree, and we can build fuzzing corpus based on this. I applied this technique for fuzzing Linux Camera Driver and Windows Video Driver, and I was able to increase the code coverage and uncover certain bugs.

#### Looking for Bug Pattern in Security Incidents

I learned this technique from a seasoned vulnerability researcher on a podcast. Although I cannot scientifically explain why it works, it has proven effective. Often, there is a bug lurking near a newly discovered one. While this may sound improbable, I successfully identified three bugs in the FastRPC code and two high-severity bugs in the Automotive Graphics Driver

#### Windows IOCTL Driver Fuzzing

One of the fascinating aspects of this project was that we tested 78 drivers without reading a single line of code, using a black-box approach, and managed to find 37 critical vulnerabilities. The fuzzing algorithm we employed observed the return values of the functions being fuzzed. Based on these observations, we assessed the coverage and validity of the IOCTL commands and the command buffer. This method provided an inexpensive way to gather feedback from the fuzzing target. Additionally, I researched several open-source projects and integrated some of their unique features into our project.

### Building Trusting Relationships

Proficiency Rating: 3

I would routinely reach out to the team leads and the other folks of the team while I was working with of WoS WiFi Driver & PMIC team to check if they need help with the project we are collaborating on or with the CR’s I reported them.

I would also ask for suggestion & feedback from QPSI member about the challenges I would face with my projects.

### Decision Making

Proficiency Rating: 3

The Windows IOCTL Interface Fuzzing project began with the simple goal of observing return values for fuzzing feedback. After conducting further research in the open-source space, I integrated additional features into the fuzzer. Moreover, I made the fuzzing process more scalable and automated, which proved invaluable as we started encountering crashes, necessitating system reboots to do continuous fuzzing.

To systematize the project’s KPIs, I needed to organize and store fuzzing progress data effectively. To address this, I integrated the Protobuf library into the fuzzer. This allowed it to store all Qualcomm driver data, along with the corresponding scanned results, IOCTL command metadata, and fuzzing outcomes, in a Protobuf file. This file facilitated the sharing of crash results with the relevant tech teams.

### Getting Work Done

Proficiency Rating: 3

The Android driver fuzzing project was challenging due to several roadblocks, such as a complex fuzzing setup, the obscured nature of user-kernel communication, and inaccurate coverage information. By breaking down each task into smaller, manageable tasks, conducting thorough research, and seeking assistance from other tech teams, I was able to make significant progress on the project.

### Mentoring and Coaching

Proficiency Rating: 3

As part of the Developer Review Training program, I have trained numerous Staff Engineers and senior-level Engineers from various teams to become DR Trainers. This initiative helps us effectively distinguish between valid and false positive bugs reported in Klockwork.

### Technical Knowledge

Proficiency Rating: 3

I have significantly enhanced my understanding of the overall System on Chip (SoC) environment, particularly in how different sub-systems interact and how data flows through these sub-systems. My knowledge of kernel drivers for both Windows and Linux has also deepened, especially in the context of fuzzing the kernel to identify vulnerabilities. I am committed to continuously expanding my expertise by reading research papers and vulnerability reports published by vendors and researchers. I actively attempt to replicate these findings to better understand the underlying issues and potential solutions.

## 2022
### People

#### QPSI
- Jyotsna Krishnaswamy - QDI Fuzzing and eAI Driver
- Can Acar- QDI Fuzzing and eAI Driver
- Aleksandr Tarasikov
- Nilo - Hypervisor binary and MSPP binary loading project

#### Non-QPSI
- Ravi Shankar Ojha -  EVS vocodec
- Gaurang Parikh - QDI Fuzzing 
- Xiaoming Zhou - eAI Driver Fuzzing
- Saravana - WIN7 Syzkaller Fuzzing Project (Chennai)
- Sukesh Chandra - Linux syzkaller project
 
### Goals

- Ozymandias - run it on modem mobs
	- NR 5g mob 
	- MPSS
- Klockwork issues SP
- eAI Driver Fuzzing

### Contributions

Project 
1.	QDI Fuzzing
	a.	Fuzzed 8 Drivers
	b.	File 7 CR’s
	c.	Found 1 Critical Bug that could lead to sandbox escape.
2.	eAI Driver Fuzzing
	a.	Fuzzed eAI Machine learning Driver
	b.	Found around 55,000 unique crashes in AFL
	c.	Filtered it down to 2000 exploitable crashes
	d.	File 48 CR’s and more to come.
	e.	2 file processing challenge with conventional fuzzer.
	f.	Working with the tech team to address the whole project Differently
3.	EVS vocodec Fuzzing
	a.	Fuzzed two version of the project 12.12 and 12.15
	b.	Found 3 bugs and file corresponding CR’s
	c.	Found it by Gap Analysis
4.	Android Multimedia
	a.	The project involved fuzzing audio codec
	b.	Found various issues in multimedia codec
	c.	Filled 8 CR’s so far
	d.	Working with to confirm other vulnerabilities and will file more CR’s


### Projects

1. [[QDI Driver Fuzzing]]
2. [[eAI Driver Fuzzing]]
3. [[EVS vocodec Fuzzing]]
4. [[Codec Fuzzing]] - OMX Integration (Android Multimedia)
5. [[Router WIN 7 - Vulnerability Detection]]
6. Auto HLOS Driver fuzzing
7. [[Lassen]] - Modem on x86
8. Long-term Fuzzing Strategy (Murali's project)
	- Off-target vs On-target
	- Goals and metrics

### Work Logs

#### QDI Driver Fuzzing

#### Idea 1
**Date** : 1/8/2021

Since I have been working on this project for past couple of weeks I have suggest few pointers which can bring some improvement to the project.
1.  Using rand(seed) to get random data is not effective, as the seed value is taken from input file which is good as we get same rand seq for each execution, but if a mutation done on the seed value, the returned value from rand calls will change for all the rand calls. Not really sure if this is will be good.
2.  I have create new library (poison_mem.c) which abstract out work of reading fuzz data file and gives you nice set of API’s to get chunks of data from fuzz file and also adds validation around dealing with the fuzz file.
	1.  This Library has will help us to make the harness more readable and eliminates the headache of tacking used/unused memory tracking.
	2.  It has _improve the execution speed of AFL from <100 exec/sec to 600-700 exec/sec_. (Earlier harness use to allocate 10 mb of memory and read the input seed file of 1024 bytes and fill that memory and poison it. For ASAN more process memory allocation means less execution speed as it has to allocate lots of shadow memory for that. Now this new library allocates only the memory as much as it is needed which is based on input file.
3.  Right now when we starts fuzzing we will create one random seed file of certain fixed size(1024 bytes) then we put our trust on the magic of mutation to figure-out the rest and then go about creating other binary input file by doing some binary hacking. To make the process more efficient I have added on script fuzz_gen.py some slices which generate seed data based on very simple grammar which I have observed in different drivers. This will bump the coverage of your driver from your first execution. One can modify this script for their own driver and store it in the same directory to improve in the future.
4.  I would also suggest to switch to AFL++ which has better mutation strategy and other improvement and also it has support for “libprotobuf-mutator”. This will also problem pointed out in point 5. libprotobuf allows us to create grammar of structures which will give produce better mutations. Since lot of our effort right while write harness is to create and correct the grammar libprotobuf will be better approach. “libprotobuf-mutator” this will help us to fuzz the c-struct in more effectively.
5.  Another technique to create effective seed corpus would be to record the interactive between driver and application in the live device and use that as a input corpus.

#### Idea 2
- **Critical CR in DSP** [CR Link](https://orbit/CR/3042725)
	- Ask Can Arcn for feedback reminding him of the bug in August 2022
	- the rating was raise from high to critical and the bug was blacklisted
	- its a critical bug in DSP WAIPOI release 
	- Jyoutsna comment on the bug **found a sneaky bug in the CENG driver that is made available to Unsigned PDs since Lahaina. A critical security CR was filed, which the team fixed overnight, and we hope it will make it into Waipio CS.**

#### eAI Driver Fuzzing


### Feedback Given

#### Alex
- What is the employee doing well? 
	- Alex and I worked for different project and he was very help and insightful in his technical suggestion.
	- He has been very punctual in meeting and very clear about his idea and how execute them.
	- He often at the end of the meeting with give practical advice send meeting notes about the meeting which make the meeting very productive

#### Bernard
- Bernard has recently on-boarded the team. We haven't really worked on any full blown project yet, but my interaction with him so far has been good, he is very open about discussing idea and he has very good knowledge about compilers. He seem to be very humble person.

#### Jyotsna Krishnaswamy
- I worked with Jyotsna on two projects QDI Fuzzing and eAI Fuzzing which she was leading at that time.
- Jyotsna was leading the QDI Fuzzing project and during that time I was new to Qualcomm and was on boarded in the project to fuzz the driver Jyotsna helped the new team members on board the project
- In QDI Fuzzing project was about fuzzing QURT OS driver and I and the rest of the team was not completely aware of the about the OS and DSP. She helped team to understand this new environment which further helped us to smoothly execution of the project.
- She was readily available to discuss any issues I had with the project and communicated very effectively about here ideas and suggestion

#### Sameesh Mukundan

#### Murali
- Murali has been very instrumental in getting interesting project for team  
- He has been very transparent with his feedback about my work and behavior with the team which help me to work with the organization more effectively.
- We have bi-weekly one-to-one meeting to discuss the project and any blocker I have with project. He give useful advice and PoC who is expert in the area to resolve those issues.
- He often advices me on how to create long term career goal and strategies to achieve them. This helps me to align my personal goal with the organization goals.
- He also often helps me to understand my strength and weakness and assigns me task and projects accordingly and also advices me and things I need to improve my weakness.
- Murali gives reorganization to my achievements and encouragement when needed.

