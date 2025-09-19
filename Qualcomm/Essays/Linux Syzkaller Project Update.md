---
created-date: 2024-12-15T10:00
tags:
  - "#type/meeting"
  - "#qpsi/meeting"
up: "[[Android Kernel Driver Fuzzing]]"
summary: Periodic Updates
participants: 
related:
---

## Project Review 

### Objective
The high level objective of the project was to assist the Linux security team to improve the current state of Linux kernel fuzzing, this will help to improve the security of Qualcomm's proprietary kernel drivers.

### Challenges
One of the method we use for security test is fuzzing and for that we use tool called syzkaller. We already had a setup of kernel fuzzing with Syzkaller. So the goal was to increase the code coverage of the current for the Waipio target. Improving the code coverage increases the likelihood of hitting the code which has bug. 

We started the project by analyzing camera module since it was amongst the most complex drivers we have. During the initial analysis phase we tried to figure out what were the blocker which we not allowing to increase the existing coverage. Few of the things that were uncovered were:
1. Complex Hardware initialization -  the camera drivers is dependent on other devices to be initialized in proper state before we can make calls to camera drivers
2. Device dependencies - there is lots of state dependency between different driver like camera and video, etc. so to get coverage of particular module other devices had to be set to proper state to reach that certain code sections.
3. Asynchronous code - There are some code section there is executed on kernel threads which is schedule independent of main code which makes it difficult for fuzzer to relate to the fuzz case and also coverage collection is difficult in such cases.
4. Limitation of the fuzzing tools - fuzz cases are written in syzkaller grammar, although the syscalls dependency can be expressed in the grammar but very complex device dependency cannot be modelled. There is also no provision to prioritize particular group of system call that would target particular sub-systems.
5. Complex state machine - device drivers have internal state-machine this creates fuzzing challenge because to fuzz a particular state we need to reach that state successfully without any error and then fuzz it which is difficult when the state is 3-4 level deep.

## Efforts

In light of above challenges I have created following tools :
- Created a program that will fuzz corpus of selected system calls which focus on only group of syscalls targeting particular sub-system.
- Created a custom user space library that can be integrates with the any userland program to collect kernel coverage and dump kernel coverage.
- Created tools of analyzing kernel coverage collect by above library, this program give report about the module coverage which helps to understand the progress made by fuzzing library.

In this process we found two bugs and have filed CR's :
- https://orbit/CR/3268610
- https://orbit/CR/3268537

This project is still work in progress and we are experimenting new ideas to improve the coverage. The idea is that camera tech team has created unit test cases which has good code coverage with different scenarios been tested, we will leverage this test cases to reach deeper parts of the code and fuzz it. We can later merge the unit-test case program with syzkaller. 

