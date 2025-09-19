---
up: "[[Qualcomm MoC]]"
tags:
  - "#type/qcom"
aliases:
  - kgsl
  - adreno
status: done
created-date: 13-12-2024
summary: Annual Review Writeup
---

## Intro

- The GPU is, as expected, a platform device located on the same die as the processor.  The registers are mapped into the physical address space of the processor.  There device also shares memory with the main processor; there is no dedicated memory on board for the GPU. The GPU has a built in MMU for mapping paged memory.
- Some processor variants also have a separate 2D core attached.  The 2D core is also a platform device with shared memory and a MM. While the general interface is similar to the 3D core, the 2D GPU has its own separate pipeline and interrupt.
- The core of the driver is a home-grown ioctl() API through a character device we call /dev/kgsl.   Like other GPU drivers, all the heavy lifting is done by the userspace drivers so the core set of ioctls are mainly used to access the hardware or to manage contexts:

	1. IOCTL_KGSL_DEVICE_GETPROPERTY
	2. IOCTL_KGSL_DEVICE_REGREAD
	3. IOCTL_KGSL_DEVICE_REGWRITE
	4. IOCTL_KGSL_RINGBUFFER_ISSUEIBCMDS
	5. IOCTL_KGSL_DEVICE_WAITTIMESTAMP
	6. IOCTL_KGSL_CMDSTREAM_READTIMESTAMP
	7. IOCTL_KGSL_DRAWCTXT_CREATE
	8. IOCTL_KGSL_DRAWCTXT_DESTROY

- In the early days of the driver, any memory used for the GPU (command buffers, color buffers, etc) were allocated by the user space driver through PMEM (a simple contiguous memory allocator written for Android; see drivers/misc/pmem.c).  PMEM is not ideal because the contiguous memory it uses needs to be carved out of bootmem at init time and is lost to the general system pool, so the driver was switched to use paged memory allocated via vmalloc() and mapped into the GPU MMU.  As a result, a handful of additional ioctls were put into the driver to support allocating and managing the memory.
- The specific implementation of GPU hardware is normally abstracted away by libraries such as [[OpenGL]] ES and Vulkan. These libraries implement a standard API for programming common GPU accelerated operations, such as texture mapping and running shaders. These libraries implement a standard API for programming common GPU accelerated operations, such as texture mapping and running shaders. At a low level however, this functionality is implemented by interacting with the GPU device driver running in kernel space. ^f16771
- The Adreno[[KGSL]] kernel device driver is primarily invoked through a number of different ioctl calls 
	- to allocate shared memory
	- create a GPU context
	- submit GPU commands
	- mmap - to map shared memory in to the user-space application

## GPU Shared Mappings / Memory management

- Security and isolation are also concerns for GPUs. As a shared resource used by many applications, obviously we don’t want one application to be able to access other applications' data. For this, GPUs also use virtual addresses and [memory management units](https://en.wikipedia.org/wiki/Memory_management_unit) (MMU, called IOMMU for Adreno and SMMU for Mali) to map virtual addresses to physical memory. The virtual address is chosen by the kernel mode driver and the kernel mode driver configures the GPU to use different page tables for different applications so that they are isolated. Hence, for the same physical memory page shared between the kernel mode driver and the application, there are two virtual addresses for it in two different virtual address spaces. The kernel mode driver uses the GPU’s IOMMU/SMMU to translate, while the application uses the CPU’s MMU to translate.

- For the most part, applications use shared mappings to load vertices, fragments, and shaders into the GPU and to receive computed results. That means certain physical memory pages are shared between a userland application and the GPU hardware.
	![[adreno-gpu-data-sharing.png]]
- There are **two distinct views on the same pages of physical memory**. 
	- The **first view is from the userland application**, which uses a virtual address to access the memory that is mapped into its address space. The CPU's memory management unit (MMU) will perform address translation to find the appropriate physical page.
	- The **other is the view from the GPU hardware itself, which uses a GPU virtual address**. The GPU virtual address is chosen by the KGSL kernel driver, which configures the device's IOMMU (called the SMMU on ARM) with a page table structure that is used just for the GPU. When the GPU tries to read or write the shared memory mapping, the IOMMU will translate the GPU virtual address to a physical page in memory.
- The kernel mode driver uses the GPU’s IOMMU/SMMU to translate, while the application uses the CPU’s MMU to translate.

## GPU context
- **Each userland process has its own GPU context**, meaning that while a certain application is running operations on the GPU, the GPU will only be able to access mappings that it shares with that process. This is needed so that **one application can't ask the GPU to read the shared mappings from another application**.
	- In practice this separation is achieved by changing which set of page tables is loaded into the IOMMU whenever a GPU context switch occurs. A GPU context switch occurs whenever the GPU is scheduled to run a command from a different process.
- However certain mappings are used by all GPU contexts, and so can be present in every set of page tables. 
	- They are called global shared mappings, and are used for a variety of system and debugging functions between the GPU and the KGSL kernel driver. 
	- While they are never mapped directly into a userland application (e.g. a malicious application can't read or modify the contents of the global mappings directly).
	- they are mapped into both the GPU and kernel address spaces.
	- we now know that the scratch buffer is correctly randomized (at least to some extent), and that it is a global shared mapping present in every GPU context.
	- What exactly is the scratch buffer used for?

## Command buffer management

Aside from supervising memory, the kernel mode driver also manages the main command buffer, which is the buffer really shared with the GPU processor. It’s commonly a ring buffer, still in system memory. We know that a ring buffer has a read pointer and a write pointer. The read pointer is used by the GPU processor for fetching the next commands for execution; the write pointer is used by the kernel mode driver for queuing more workloads.

Command buffers created by a specific application are just placed in normal system memory allocated to the user mode driver; the kernel mode driver needs to copy them to the ring buffer. This allows many applications to prepare workloads simultaneously without always contending for some lock. Well, actually not copy as the application generated command buffers can be large. So the kernel just inserts “indirect calls” into those command buffers in the ring buffer.

So in summary in ring buffer, we can see GPU setup and teardown commands, context switching commands, and “indirect calls” to application-specific command buffers. There are also others (e.g., for performance counters) that I won’t go into details.

## Scratch Buffer

- Scratch buffer are allocated in the driver's probe routines, meaning that the scratch buffer will be allocated when the device is first initialized:

```C
int adreno_ringbuffer_probe(struct adreno_device *adreno_dev, bool nopreempt) {
...
status = kgsl_allocate_global(device, &device->scratch,
                            PAGE_SIZE, 0, KGSL_MEMDESC_RANDOM, "scratch");
```

-  The scratch memory is one page worth of data that is mapped into the GPU. This allows for some 'shared' data between the GPU and CPU. For example, it will be used by the GPU to write each updated RPTR for each RB.
- We can find two primary usages of the scratch buffer:
	1.  The GPU address of a preemption restore buffer is dumped to the scratch memory, which appears to be used if a higher priority GPU command interrupts a lower priority command.
	2.  The **read pointer (RPTR)** of the **ring-buffer (RB)** is read from scratch memory and used when calculating the amount of free space in the ring-buffer. ^f57eb3

## Ring Buffer

- To understand what an invalid RPTR value might mean for a ring-buffer allocation, we first need to describe the ring-buffer itself. When a userland application submits a GPU command (IOCTL_KGSL_GPU_COMMAND), the driver code dispatches the command to the GPU via the ring-buffer, which uses a producer-consumer pattern. The kernel driver will write commands into the ring-buffer, and the GPU will read commands from the ring-buffer.
- This occurs in a similar fashion to classical [circular buffers](https://en.wikipedia.org/wiki/Circular_buffer). At a low level, the ring-buffer is a global shared mapping with a fixed size of 32768 bytes. 
- Two indices are maintained to track where the **CPU is writing to (WPTR)**, and where the **GPU is reading from (RPTR)**. To allocate space on the ring-buffer, the CPU has to calculate whether there is sufficient room between the current WPTR and the current RPTR. ^gpu-circular-buff
- This happens in **adreno_ringbuffer_allocspace**: ^aderno-gpu-code
	```C
	unsigned int *adreno_ringbuffer_allocspace(struct adreno_ringbuffer *rb,
	                unsigned int dwords)
	{
	        struct adreno_device *adreno_dev = ADRENO_RB_DEVICE(rb);
	        unsigned int rptr = adreno_get_rptr(rb); [1]
	        unsigned int ret;
	        if (rptr <= rb->_wptr) { [2]
	                unsigned int *cmds;
	                if (rb->_wptr + dwords <= (KGSL_RB_DWORDS - 2)) {
	                        ret = rb->_wptr;
	                        rb->_wptr = (rb->_wptr + dwords) % KGSL_RB_DWORDS;
	                        return RB_HOSTPTR(rb, ret);
	                }
	                /* 
	                 * There isn't enough space toward the end of ringbuffer. So
	                 * look for space from the beginning of ringbuffer upto the
	                 * read pointer.
	                 */
	                if (dwords < rptr) {
	                        cmds = RB_HOSTPTR(rb, rb->_wptr);
	                        *cmds = cp_packet(adreno_dev, CP_NOP,
	                                KGSL_RB_DWORDS - rb->_wptr - 1);
	                        rb->_wptr = dwords;
	                        return RB_HOSTPTR(rb, 0);
	                }
	        }
	        if (rb->_wptr + dwords < rptr) { 
	                ret = rb->_wptr;
	                rb->_wptr = (rb->_wptr + dwords) % KGSL_RB_DWORDS;
	                return RB_HOSTPTR(rb, ret); [4]
	        }
	        return ERR_PTR(-ENOSPC);
	
	}
	
	unsigned int adreno_get_rptr(struct adreno_ringbuffer *rb)
	{
	        struct adreno_device *adreno_dev = ADRENO_RB_DEVICE(rb);
	        unsigned int rptr = 0;
	[...]
	                struct kgsl_device *device = KGSL_DEVICE(adreno_dev);
	                kgsl_sharedmem_readl(&device->scratch, &rptr,
	                                SCRATCH_RPTR_OFFSET(rb->id)); [5]
	[...]
	        return rptr;
	}
	```
- We can see the RPTR value being read at [1], and that it ultimately comes from a read of the scratch global shared mapping at [5]. Then we can see the scratch RPTR value being used in two comparisons with the WPTR value at [2] and [3]. The first comparison is for the case where the scratch RPTR is less than or equal to WPTR, meaning that there may be free space toward the end of the ringbuffer or at the beginning of the ringbuffer. The second comparison is for the case where the scratch RPTR is higher than the WPTR. If there's enough room between the WPTR and scratch RPTR, then we can use that space for an allocation.
- So what happens if the scratch RPTR value is controlled by an attacker? In that case, the attacker could make either one of these conditions succeed, even if there isn't actually space in the ringbuffer for the requested allocation size. For example, we can make the condition at [3] succeed when it normally wouldn't by artificially increasing the value of the scratch RPTR, which at [4] results in returning a portion of the ringbuffer that overlaps the correct RPTR location.]
- The idea behind [[fencing]] is to ensure ordering between operations—in particular to synchronize buffer sharing between the GPU and display drivers. It will make sure that the GPU does not write to a buffer that is still being displayed and that buffers won't be displayed while the GPU is still rendering to them. It allows the display driver and the GPU to operate asynchronously.

## Performance Counter

But, why looking into perf counters? What’s interesting about them? Perf counters are special processor registers showing various metrics about the processor. They are crucial for helping understanding software performance issues and making the best use of hardware.

## Known CVE

1. CVE-2020-11239 [writeup](https://securitylab.github.com/research/one_day_short_of_a_fullchain_android/)

## Existing Research

- [The code that wasn’t there: Reading memory on an Android device by accident](https://github.blog/2023-02-23-the-code-that-wasnt-there-reading-memory-on-an-android-device-by-accident/)
	- Memory Leak Bug in Qualcomm GPU Driver
- [An Exploit Chain to Remotely Root Modern Android Devices](https://github.com/secmob/TiYunZong-An-Exploit-Chain-to-Remotely-Root-Modern-Android-Devices/blob/master/us-20-Gong-TiYunZong-An-Exploit-Chain-to-Remotely-Root-Modern-Android-Devices-wp.pdf) ^qcom-gpu-bug-1
	-  CVE-2019-10567 (June 2020)
	- This is a logical bug in Qualcomm Adreno [[GPU]] Driver
- [Attacking Adreno GPU Google Project Zero](https://googleprojectzero.blogspot.com/2020/09/attacking-qualcomm-adreno-gpu.html)
	- CVE-2020-11179 (November 2020)
	- This is a logical bug in Qualcomm Adreno [[GPU]] Driver
	- this is a follow on research of [[#^qcom-gpu-bug-1]]
	- **Execute user profiling in an indirect buffer(IB)**. This ensures that addresses and values specified directly from the user don't end up in the ring-buffer.
	- why exactly is it important that user content doesn't end up on the ring-buffer
	- what happens if we can recover the base address of the scratch mapping
	- So what happens if the scratch RPTR [[Adreno GPU Internal#^gpu-circular-buff]] value is controlled by an attacker? In that case, the attacker could make either one of these conditions succeed, even if there isn't actually space in the ring-buffer for the requested allocation size. For example, we can make the condition at [[Adreno GPU Internal#^aderno-gpu-code]] succeed when it normally wouldn't by artificially increasing the value of the scratch RPTR, which at [4] results in returning a portion of the ring-buffer that overlaps the correct RPTR location.]
- [Android GPU drivers](https://github.blog/2022-06-16-the-android-kernel-mitigations-obstacle-race/)
	- [[UAF]] bug, [[CVE-2022-22057]] in the Qualcomm [[GPU driver]]
- [Corrupting memory without memory corruption](https://github.blog/2022-07-27-corrupting-memory-without-memory-corruption/)
	- A vulnerability in the Arm Mali GPU that I reported to the Android security team on 2022-07-12
- [Pwning the all Google phone with a non-Google bug](https://github.blog/2023-01-23-pwning-the-all-google-phone-with-a-non-google-bug/)
- [The code that wasn’t there: Reading memory on an Android device by accident](https://github.blog/2023-02-23-the-code-that-wasnt-there-reading-memory-on-an-android-device-by-accident/)
- CVE for Android Adreno GPU Linux kernel driver
	- CVE-2019-10567
	- CVE-2020-11179
	- https://www.exploit-db.com/exploits/39504
	- 
## Ref
- https://lwn.net/Articles/394665/
- [Securing GPU via Region-based Bounds Checking](https://dl.acm.org/doi/pdf/10.1145/3470496.3527420)
- [Understanding The Security of Discrete GPUs](https://dl.acm.org/doi/pdf/10.1145/3038228.3038233)
- https://www.lei.chat/posts/android-linux-gpu-drivers-internals-and-resources/
- https://www.lei.chat/posts/sampling-performance-counters-from-gpu-drivers/
- [The Mali GPU: An Abstract Machine](https://community.arm.com/arm-community-blogs/b/graphics-gaming-and-vr-blog/posts/the-mali-gpu-an-abstract-machine-part-1---frame-pipelining)