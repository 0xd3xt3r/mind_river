---
aliases:
  - SoC Overview
  - Advenctures of SoC
  - ARM SoC Overview
  - SoC Reverse Engineering
tags:
  - reverse-engineering
  - firmware-reversing
  - bare-metal-firmware
  - "#type/knowledge"
status: todo
created-date: 2024-12-15
up: "[[Knowledge Base MoC]]"
related:
summary: understand low-level components
---


# Notes
- You will encounter lots of abbreviations make a not for their full form and a short description of their function. Make this as a small dictionary to read the Reference Manual. This will come in handy because you will encounter loads of this terms and if you don't understand them you will be very frustrated as the manual will be useless without them.
- [How to read datasheets](https://www.youtube.com/watch?v=1EXXqWweTkI&ab_channel=Phil%E2%80%99sLab)

# MMU

- ARM v8 - 64 bits 
- It support 3 different page granule size which is of 64kb, 16kb and 4kb 
- MMU has registers to point to base address of the process. ARM uses two register one for Kernel(TTBR1) and user process (TTBR0) and the behaviors of the page table translation is configured using TCR register.
- MMU also allows you to specify range of the Kernel space and User space using TCR Register using T0SZ for Userspace and T1SZ using Kernel space. *TCR_TxSZ* specifies the number of bit mask for the range of kernel and user space process.
- most significant bits of the input VA that determine whether TTBR0 or TTBR1 holds the required translation table base address.
- Things need to translate
	- number of bit in VA to used for translation for eg 42,52 etc.
	- which register TTBR0 or TTBR1
	- what is the granularity of each page size
	- Table entry format and which bit determine PA and page attributes
	- Page tables have section, super section, etc. 
		- How to determine this?
- Translation Control Register (TCR)
	- ![[Pasted image 20240219142520.png]]
	- TG1, bits - describes the granule size
		- ![[Pasted image 20240202190138.png]]
		- ![[Pasted image 20240219142612.png]]
	- AS, bit - this describes if TTRB0 or TTBR1 will hold the ASID. ASID is used in TLB to cache address translation
	- IPS bit - this decides how many bits can be used for *Virtual to Physical* address translation.
		- ![[Pasted image 20240202185758.png]]
	- the TCR_ELx.{T0SZ, T1SZ} fields configure all of the following address range sizes:
		- The lower VA range is 0x0000000000000000 to (2(64-T0SZ) - 1). 
			- This specify Userspace process address range.
			- specifies the size for the lower VA range, translated using TTBR0_EL1.
		- The upper VA range is (264 - 2(64-T1SZ)) to 0xFFFFFFFFFFFFFFFF. - Kernel space range
			- specifies the size for the upper VA range, translated using TTBR1_EL1.
			- This specific Kernel process address range.
		- ![[Pasted image 20240202195018.png]]
- [Raspberry PI implement](https://github.com/bztsrc/raspi3-tutorial/blob/master/10_virtualmemory/mmu.c)

# ARM Generic Interrupt Controller - GIC

![[Pasted image 20240105104345.png]]

As processors evolved, soon the number of interrupts became quiet large and also it was not uncommon to have more than 1 core (either symmetric or asymmetric), there was a need for a more standardized way of handling interrupts.
- **Distributor:** This is the peripheral facing component that is available as only one implementation (instance). It is responsible for managing interrupts in the whole system and decides priorities between them and routing mechanism of the same.
- **CPU Interfaces:** For each CPU core available, there is a corresponding CPU Interface present bridging the Distributor interface with the core. It implements the priority masking for the processor.

## The Distributor

The Distributor centralizes all interrupt sources, determines the priority of each interrupt, and for each CPU interface forwards the interrupt with the highest priority to the interface, for priority masking and preemption
handling. The Distributor provides a programming interface for:
- Globally enabling the forwarding of interrupts to the CPU interfaces.
- Enabling or disabling each interrupt.
- Setting the priority level of each interrupt.
- Setting the target processor list of each interrupt.
- Setting each peripheral interrupt to be level-sensitive or edge-triggered.
- Setting each interrupt as either Group 0 or Group 1.

## Interrupt IDs
- Interrupts from sources are identified using ID numbers. Each CPU interface can see up to 1020 interrupts. The banking of SPIs and PPIs increases the total number of interrupts supported by the Distributor.
- The GIC assigns interrupt ID numbers ID0-ID1019 as follows:
	- Interrupt numbers ID32-ID1019 are used for SPIs.
	- Interrupt numbers ID0-ID31 are used for interrupts that are private to a CPU interface. These interrupts are banked in the Distributor.
		A banked interrupt is one where the Distributor can have multiple interrupts with the same ID. A banked interrupt is identified uniquely by its ID number and its associated CPU interface number. Of the banked interrupt IDs:
		- ID0-ID15 are used for SGIs
		- ID16-ID31 are used for PPIs
- In a multiprocessor system:
	- A PPI is forwarded to a particular CPU interface, and is private to that interface. In prioritizing interrupts for a CPU interface the Distributor does not consider PPIs that relate to other interfaces

## CPU interfaces

Each CPU interface block provides the interface for a processor that is connected to the GIC. Each CPU interface provides a programming interface for:
- enabling the signaling of interrupt requests to the processor
- acknowledging an interrupt
- indicating completion of the processing of an interrupt
- setting an interrupt priority mask for the processor
- defining the preemption policy for the processor
- determining the highest priority pending interrupt for the processor.

When enabled, a CPU interface takes the highest priority pending interrupt for its connected processor and determines whether the interrupt has sufficient priority for it to signal the interrupt request to the processor. To
determine whether to signal the interrupt request to the processor, the CPU interface considers the interrupt priority mask and the preemption settings for the processor. At any time, the connected processor can read the priority of its highest priority active interrupt from its GICC_HPPIR, a CPU interface register. The mechanism for signaling an interrupt to the processor is IMPLEMENTATION DEFINED.

## Register Info

| Register Name | Register Info | Description|
|---|---|---|
| ICDIPTR | Interrupt Processor Targets Registers | used to specify the CPU interfaces to which each interrupt should be forwarded |
| ICDIPR | Interrupt Priority Registers | associate a priority level with each individual interrupt |

## Ref

- [Brief Into of ARM GIC](http://ecse324.ece.mcgill.ca/fall2021/_downloads/eb3bd46aeaebdd7c592e61dbc13dc1d3/Using_GIC.pdf)
- [GIC Intro](https://www.embien.com/blog/arm-interrupt-controllers)
- [Video Lecture on ARM GIC With code example](https://www.youtube.com/watch?v=scBA_C17pgQ&list=PLxfJmMEKJn4dBsyWQqM63DrHiBcvrV8my&index=18&ab_channel=%C2%AD%EC%84%9C%ED%83%9C%EC%9B%90%5B%EA%B5%90%EC%88%98%2F%EC%BB%B4%ED%93%A8%ED%84%B0%ED%95%99%EA%B3%BC%5D)

# System Memory Management Unit (SMMU) aka IOMMU

![[Pasted image 20241015171631.png]]

- In computing, an input–output memory management unit (IOMMU) is a memory management unit (MMU) connecting a (DMA-capable) I/O bus to the main memory.
- Like a traditional MMU, which translates CPU-visible virtual addresses to physical addresses, the IOMMU maps device-visible virtual addresses (also called device addresses or Memory Mapped I/O addresses in this context) to physical addresses.
- It exposes the Physical Address (like RAM) to peripherals device. This will save lot of transfer time
- Advantages
	- Large regions of memory can be allocated without the need to be contiguous in physical memory – the IOMMU maps contiguous virtual addresses to the underlying fragmented physical addresses.
	- Devices that do not support memory addresses long enough to address the entire physical memory can still address the entire memory through the IOMMU, avoiding overheads associated with copying buffers to and from the peripheral's addressable memory space.
	- Memory is protected from malicious devices that are attempting [DMA attacks](https://en.wikipedia.org/wiki/DMA_attack "DMA attack")
1. ARM SMMU Software Guide Document
	1. **What is StreamID?** : The StreamID is how the SMMU distinguishes different client devices. At its simplest, one device can have one StreamID. However, a device might be able to generate multiple StreamIDs, with different translations applied based on the StreamID. For example, for a DMA engine that supports multiple channels, different StreamIDs might be applied to different channels.

2. https://www.openeuler.org/en/blog/wxggg/2020-11-21-iommu-smmu-intro.html

## Mailbox

There is no mailbox registers part of ARM processors.

Mailbox is an IP peripheral used mainly to establish messages communications and exchange between two different CPU domain within same SOC (System On Chip).

So in case you have software entity (can be basic bare metal application, user space processor running on top of kernel, kernel process…) running in domain A that has for instance two cortex-A76 performing heavy tasks requested by this software entity, at some point this software entity may request some specific services (like security services, low power based tasks) that will be executed by domain B that has a cortex-A55 processors, to send this request we should have a mechanism to dispatch this message request from domain A to domain B and of course get the answer from domain A to domain B, one trivial idea here is to use the shared memory that both cluster processors can access, the problem with such approach is to synchronize the access between both domain processors to avoid for example any kind of race condition, also domain B should read message when domain A has written it so we should have a way to signal that and same when reading back the answer from domain A, this is known and producer/consumer design pattern. The mailbox or messaging unit is the solution for this problem by creating hardware based signaling peripherals to signal the presence of message either to domain A or B and tag each message to be send and received.

ARM processors by themselves doesn’t have a peripheral called mailbox like it is defined above, however as ARM doesn’t provide a single core design but whole SOC design solution, for instance can provide whole cluster that can have up to 4 ARM processor , or whole interconnect that will connect two different ARM based domains, in such scenario they have already hardware based solution to synchronize communication between those processor:

in Multicore cluster, to switch execution from one processor to another they use the GIC (Generic Interrupt Controller) to perform such switching, in GIC there are dedicated type of interrupts called SGI (Software Generated Interrupts) where you specify in one GIC register the CPU or PE (Processing Element) to jump to then GIC perform the switch and target core within the same cluster will start execution, this is type of intercommunication between CPUs in same cluster performed by GIC controller.
Cache Coherency is one of the major problems in Multi-core/multi-domain based architecture that support caching, and to speed up cache coherency lookup then ARM have added hardware support for such cache lookup and snooping of cached data within different caches in the system, so within the same cluster and to search and snoop cached instruction within each CPU L1 cache we the SCU or Snooping Coherency Unit and this provide hardware mechanism to pass and exchange cached instruction between different cores, they have as well CCI-400 (cache Coherency Interface 400) for cached data snooping and exchange across domains not only within same domain, they some messages called DVM (Distributed Virtual Messages) for snooping MMU Translation table entries within TLB (Translation Lookaside Buffer), AMB5 Bus as well has this cache coherency support, AXI-ACE (AXI Coherency Extension) has extra channels added for cache data coherency transfer.
So those ARM embedded messages exchange based hardware IPs are added mainly for hardware cache coherency support or to switch between cores during execution, but as plain mailbox communication this is different and we need a dedicated peripheral mailbox that is not embedded or ARM processors.

# ARM [[CoreSight]] [[Debug]] and Trace Infrastructure

- CoreSight provides debugging and tracing of single as well as multiple processor systems and even other design blocks that are not processors (e.g., Mali GPU).
- Different type of debug components in CoreSight. These are:
	- A *Debug Control Block* inside the processor core
	- *Breakpoint Unit (BPU)* - but, for historical reasons, also called the Flash Patch and Breakpoint unit "FPB")
	- *Data Watchpoint and Trace (DWT)* -  can be used to generate selective data trace, profiling trace and event (i.e., exception) trace. It has PC Sampler, Data tracer, Interrupt tracer and ETM Trigger.
	- *Instrumentation Trace Macrocell (ITM)* This unit allows software to generate debug messages (e.g., printf, RTOS aware debugging support).
	- *Embedded Trace Macrocell (ETM)* - This provide instruction trace. When enabled, the program flow information (i.e., instruction trace) is generated in real-time and is collected by a debug host via the parallel trace port on the TPIU (Trace Port Interface Unit). Because the debug host is likely to have a copy of the program image, it is able to reconstruct the program execution's history.
		- Trace data can be stored in system memory instead of send it debug connection (TPIU). MTB and ETB are two method of storing trace buffer. Which are as follows
		- *Micro Trace Buffer (MTB)* - This solution uses a small portion of on-chip SRAM to hold the instruction trace data. When MTB trace is not in use, this unit works as a normal SRAM memory.
			-  The trace data is stored in a user configurable area of RAM at runtime. External debug tools can be used to start or stop the trace. The size and location of the RAM storage is configurable in software, allowing the resources to be reclaimed from a development build when they are no longer needed. 
			- The MTB can be programmed by the debug tools to cause the processor to enter a halt state when the buffer becomes full.
			- This was the first version of the buffer solution and its used especially in Cortex-M series of SoC.
			- The MTB supports two operation modes:
				- *Circular buffer mode* - the MTB uses the allocated SRAM in circular buffer mode. The MTB trace operates continuously, with the old trace data being constantly overwritten by the new trace data. 
					- If the MTB is used for software failure analysis (e.g., HardFault), the debugger set's up the processor, using the Vector Catch feature, so that it enters halt automatically when a HardFault occurs. When the processor enters HardFault, the debugger extracts the information in the trace buffer and recreates the trace history.
				- *One shot mode* - the MTB starts writing trace from the start of the allocated trace buffer and automatically stops tracing when the trace write pointer reaches a specific watermark level. The MTB can, optionally, stop the processor's execution by asserting a debug request signal.
		- *Embedded Trace Buffer (ETB)* - this uses a *dedicated SRAM* to hold trace data which has been generated by various trace sources and which has been transferred by the trace bus. This is new revision of MTB solution. 
			- The ETB provides real-time full-speed storing capability, but is limited in size. Triggering is supported for events such as buffer full and acquisition complete.
		- *Program Trace Macrocell (PTM)* - The PTM is the block for tracing processor execution flow. It is based on the Arm program flow trace (PFT) architecture, and is a CoreSight component of the trace source class. The PTM provides some general resources, such as address/ID comparators, counter, sequencers, for setting up user-defined event triggering conditions. This enhances the base functions of tracing program execution. 
	- *Embedded Cross Trigger (ECT)* - is the cross-triggering mechanism. Through ECT, a CoreSight component can interact with other components by sending and receiving triggers. ECT is implemented with two components:
		- *Cross Trigger Interface (CTI)* -  A CTI listens to one or more channels for an event, maps a received event into a trigger, and sends the trigger to one or more CoreSight components connected to the CTI. A CTI also combines and maps the triggers from the connected CoreSight components and broadcasts them as events on one or more channels.
		- *Cross Trigger Matrix (CTM)* - One or more CTMs form an event broadcasting network with multiple channels. CTI is interconnected via CTM.
		- Both CTM and CTI are CoreSight components of the control and access class.
	- *Cross Trigger Interface (CTI)* - For debug synchronization in multicore systems.
		- Provides a interface to enable from various sources to trigger debug and trace activity.
	- *Trace Port Interface Unit (TPIU)* - is used to output trace packets to the external world in order that trace data can be captured by trace capturing devices, e.g., like Keil ULINKPro.
- After being enabled by a debugger, the trace sources operate independently. In order to enable correlation between the different trace streams, a timestamp mechanism is provided.
- Tracing and Debugging are two different type of technique which can assist in debugging a program. Tracing is a 
- **CoreSight Discovery**: The identification of debug components
	- CoreSight Debug Architecture is very scalable and can be used in complex System-on-Chip designs with a large number of debug components. In order to support a wide range of system configurations, CoreSight Debug Architecture provides a mechanism to allow debuggers to automatically identify debug components in the system. This involves the use of ID registers inside each debug component and one or more lookup ROM table(s).
		- After confirming the AP module is an AHB-AP, the debugger identifies the base address of the primary ROM table by reading one of the registers inside the AHB-AP. This register contains the base address, which is a read-only value.
		- Using the ROM table base address obtained in step 5, the debugger reads the ID registers of the primary ROM table and confirms that it is a ROM table.
		- The debugger reads though the entries in the ROM table to collect the base addresses of the debug components and, if it is available, an additional ROM table. The ROM table contains one or more debug components entries

## Acroniums 

- ETM - Embedded Trace Microcell
- MTB - Micro Trace Buffer
- DWT - Data Watch trace
- ITM - Instruction Trace Microcell
- TPIU - Trace Port Interface Unit
- ATB - Advanced Trace Bus
- 

## Ref
- [Definitive Guide to Arm Cortex-M23 and Cortex-M33 Processors - CoreSight Chapter in the Book](https://learning.oreilly.com/library/view/definitive-guide-to/9780128207369/B9780128207352000160.xhtml#s0025)
- [Blog about how to use MTB along with code example](https://interrupt.memfault.com/blog/instruction-tracing-mtb-m33)


## Bus Architecture - AMBA (Advanced Microcontroller Bus Architecture)

![[Pasted image 20240110153826.png]]

1. **AHB:** Advanced High Performance Bus
2. **ASB:** Advanced Peripheral Bus
3. **APB:** Advanced Peripheral Bus
4. **ATB:** Advanced Trace Bus
5. **AXI:** AMBA Extensible Interface

- [Overview of AMBA](https://medium.com/@iamRadhaKulkarni/understanding-the-amba-protocol-apb-ahb-and-asb-explained-29cc68540113)