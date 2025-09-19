---
tags:
  - archived
---

## Camera Fuzzing Report

**Project Title :**

Fuzzing Qualcomm’s Linux Driver with Syzkaller

**Project Description**

The goal of the project is to improve the current state of code coverage of the Qualcomm’s driver for Android devices. We are currently focusing on improving the coverage of camera driver and then move on to other modules. After investigating the syzkaller coverage the reason for very low coverage is that camera is very complex driver and its has very stateful code. So to even reach certain section of code we need to put the driver in proper state. To put the driver is particular state requires sequence of syscalls which requires deep knowledge of camera module. To mitigate this problem I came up with the idea to repurpose our existing camera module unit-test cases to uncover sequence of syscall done to execute certain use-case and then intercept the syscall and mutate the data sent to the kernel.

**Project report**

I am able to fuzz the sensor sub-system of camera driver with the above method and get coverage of about 900 basic block. This area was uncovered by previous method of fuzzing.

**Technical Details**

To achieving this following things were done

1.  Modifying the existing unittest code to that I can provide fuzzing data for the call which are influenced by user input.
2.  Patched the unittest binary to make is shared library and exported fuzzing function symbols.
3.  Modified the syzkaller executor to integrate the unit-test case library so that we can take mutation data from syzkaller and see the coverage data on syzkaller dashboard.

## Windows Fuzzing Report

**Fuzzing Qualcomm’s Video Driver in Windows ARM**

**Project Description**

The goal is to create fuzzer for video driver on windows on ARM. First step was to figure out the attack surface, since its about attacking Windows Graphics kernel sub-system it isn't like Linux where you have standard system calls and documentation. Next setup was to figure who to trigger those API call from userland program so that we can start inject fuzzing data in the kernel. The next step is the create fuzz corpus. But its not straight forward as AFL where you have API where you fill the fuzzing data and expect to crash. For this project we have do something called as property based fuzzing where we want to test the sequence of the API calls along with fuzz API parameter because this what user can controls. Gather code coverage is another problem we I haven't put any effort into it yet as I am focusing on solving above problems.

**Project report**

After studying Windows DirectX sub-system and Qualcomm Driver I manage to understand the attack surface and create a map for what all part we can attack via an API call from userland. This was also assisted by looking at past research done for this area. Doing internal code review and through Microsoft documentation I was able to trigger some of the API's of our attack surface. To generate corpus data using PRNG to generate continues sequence of random data and scoop out needed data which feed it to the function call and also used the same sequence to decide which sequence of function call should we make. This will randomized both API sequence and API parameter. Code coverage is I have seen some QDSS function which are doing what we need but I still need to figure out how to use it.

**Technical Details**
Below are some of the challenges that I am anticipating:
1. There are no de-facto kernel fuzzer tool in open source community that support windows so we might how to look for whatever option is available or create one or port syzkaller
2. Gather code coverage is another challenge there are couple of options like bullseye or ARM CoreSight which is integrate as part of QDSS sub-system
3. Seed Data : an obvious solution might be to just play the video file but this might slowdown the fuzzer