---
up: "[[Learning MoC]]"
tags:
  - "#type/video-stream"
status: todo
created-date: 10-12-2024
media_link:
---

## Annotations

1. 

## Notes

## Android Exploit

^d0c01f
In this project [[gamozolabs]] exploit a bug Android Kernel to get kernel execution. Following that he write exploit to dump the kernel memory and rehost it on Qemu Emulator.

### Commands

1. Configure the build - `./configure --target-list=arm-softmmu --enable-tcg-interpreter --static`
2. running qemu - `./qemu-system-arm -machine none -cpu cortex-a9 -nographic -m 1 -singlestep -accel tcg,tb-size=1 -device loader,file=<finename>,addr=0,force-raw=on`
	1. '-S' - freeze CPU at startup
	2. '-s' - setup gdb
 2. `rasm2 -a arm -D -B -f hi.bin -l 10`
 3. `rasm2 -a arm -b 32 'push {r2, r3, r7}'`

###  Video Annotations
1. [Dumping register and physical memory state with our Android exploit](https://www.youtube.com/@gamozolabs)]
	1. 2:15:00 - video start from here discussing the goals of what needs to be done.
		1. Halt the kernel by disabling all the interrupts
		2. Get and Save all the registers
		3. Save all the physical memory to file
2. [Reading Kernel Page table](https://www.youtube.com/watch?v=NJjpkzuc1k4&list=PLSkhUfcCXvqFfZLRrghKwWsULxYeAIuhf&index=4&ab_channel=gamozolabs)
	1. In this stream the kernel memory is dumped on the file system and he is parsing the dump and using register values to walk the page table
	2. 1:46:00 - start parsing ttbr0 register
3. [Android exploit snapshot running in QEMU (Part 1/2)](https://www.youtube.com/watch?v=6TzdYokXoF8&list=PLSkhUfcCXvqHpQ1aL-HZzZvOxY5L2F_NA&index=11&ab_channel=gamozolabs) - In this stream we finished getting our snapshotting done and loaded and resumed execution of the phone in QEMU, allowing for debugging and stuff!.
	1. 3:30:00 - compiling QEMU
	2. 3:58:00 - running simple QEMU ARM machine.
	3. 4:15:00 - simple modification on TCG
	4. 4:25:00 - loading ram snapshot
	5. 5:14:09 - enable paging
	6. 5:39:00 - paging enabled successfully
	7. 5:43:00 - gdb attached to the Qemu
4. [Android exploit snapshot running in QEMU (Part 2/2)](https://www.youtube.com/watch?v=hlW8ktQkyPA&list=PLSkhUfcCXvqHpQ1aL-HZzZvOxY5L2F_NA&index=10)
	1. previous video continues
5. [Trying to figure out how we want to fuzz with QEMU](https://www.youtube.com/watch?v=2K3DnMxNOrI&ab_channel=gamozolabs) - In this video we go through the thought process of figuring out if we want to use QEMU for fuzzing this Android device, or if we want to write a custom emulator.
	1. 3:46:00 - start navigating TCI code to understand emulation
	2. 5:13:00 - discovered the point at which virtual memory writes can be hooked.