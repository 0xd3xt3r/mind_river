# Embedded Systems Security

Tags: #firmware-reversing , #bare-metal-firmware 

## [[Code Emulation]]

Emulation can be done at different level. From full system to partial code this depends on your code and objective.

1. [[Full System Emulation]]
	1. Emulates every aspect of system from boot, to kernel, to user land application.
	2. Very powerful as you can instrument every instruction of the system from user-land to kernel.
	3. You can do taint analysis of every bit of data from user-land to kernel.
	4. But of-course all this comes at computational cost.
	5. A good example will be tool like Qemu, [panda.re](https://github.com/panda-re/panda)
2. [[User mode Emulation]]
	1. In this technique you want to abstract out the Kernel and you want the kernel to handle the system call.
	2. You have no control of Kernel execution and you control only of the data which is passed to Kernel.
	3. Is little faster then Full System Emulation
3. [[Partial Emulation]]
	1. You want to run only the very specific section of the binary it could be a function or group of functions.
	2. Very fast compared to other method but its difficult to setup the partial code so that it can execute.
	3. You use this technique when you have very clear goal of what you want to achive and then you can do very close monitoring.

## [[Secure Boot]]
1. The goal of secure boot is to run and load only the authenticate code in your system/processor.
2. The bootloader should have minimum code, should contain only flash support, cryptographic primitive and optional recovery mechanism.
3. Should use asymmetric cryptographic to verify the firmware integrity.


### Resources
1. [Secure boot with wolfBoot (wolkSSL folks)](https://www.youtube.com/watch?v=u5sP31WHy_o)
2. https://www.bytesnap.com/secure-boot-imx6-part-1/#


## Reference

1. [Embedded Systems Security and TrustZone](https://embeddedsecurity.io/index.html)
2. Firmware Reversing Blog - https://blog.3or.de
3. [Software Updates for Internet of Things (SUIT)](https://datatracker.ietf.org/wg/suit/documents/)