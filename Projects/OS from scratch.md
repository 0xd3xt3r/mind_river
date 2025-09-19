---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/os"
status: abandoned
created-date: 2024-12-27
related:
  - "[[Intel Firmware Attack & Defense]]"
  - "[[Rootkits and Bootkits]]"
summary: This project is about creating bear minimum operating system kernel up and running.
---

# OS From Scratch

This project is about creating bear minimum operating system kernel up and running.

## Fundamental Concepts

Some very important fundamental concepts which you need to know in order to have a very basic kernel running

### Bootloader

This is the piece of code which will load the Operating system kernel in the Main memory.

###  Segmentation

![[Pasted image 20210329141259.png]]

#### Segment Selector

[[Segment Selector Register]] are :
1. **Code Segment (CS)** - all code access implicitly use CS register for address translation, for example *[[EIP]] == CS:EIP*
2. **Data Segment (DS)** - same as above but for Data access.
3. **Stack Segment (SS)** - same as above but for Stack data access. for example *[[ESP]] == SS:ESP*
4. Other segment register are, General Segment (GS), FS , ES which are extra could be used you we wish.
	1. Window stores [[Thread Environment Block]] [[TEB]] in FS:[0] register. Linux the similar information in Creating OS from scratch [[Thread Local Storage]] [[TLS]] in GS:[0]

#### Segment Descriptor
1. These register points in Segment Descriptor Table which could be [[Global Description Table]] [[GDT]] or [[Local Description Table]] [[LDT]]. This value are stored in register call GDTR and LDTR respectively.
1. Segment selector has hardware cache which programmer cannot access but its used to cache linear address.
2. [[LDTR]] and [[GDTR]] can be read from user-space and are used as [[Anti-VM]] technique by the malware.
3. The access LDT you have to go through GDT entry. So effectively [[LDTR]] pointer to entry in GDT which has pointer to entry in LDT.  

#### Address Translation

The address is translated from **[[Logical Address]]** -> **Linear Address** and if *paging* is enabled the **[[Linear Address]]** is then translated to **[[Physical Address]]** which is on the RAM memory.

on 32-bit system the address ranges from 0000-0000 to FFFF-FFFF which is 4 GB but if the **[[physical address extension]] (PAE)** is enabled then the architecture can support upto (36 bit addressing 2^36 )64 GB of memory.


#### Paging

Paging is mechanism of translating [[Linear Address]] to physical memory this mode have to be turned on by setting xyz bit in the CRx register . This mode of addressing used when you want to very less RAM say 256 MB and still want to address using 32-bit addressing.

![[Pasted image 20210329141147.png]]

1. Linear address are translated to physical address using transition table
2. There are control registers CR0-CR4 which facilitated paging
3. CR0 has PG bit if set enables [[paging]], and PE bit if set enables [[protected mode]]. For paging PE and PG both are required to be set.
4. CR2 register stores the [[linear address]] which we CPU was trying to translate caused [[page fault]] and NOT the instruction running at the time.
5. CR3 points to the [[page directory]] base.
6. CR4 register has flag like Physical Address Extension, Page Size Extension and Page Global Enable.
	1. PGE - this marks certain linear address translation entry in the cache as global so, when the process switch takes place those entries in the cache are not removed. This features is good translating for kernel code as the address mapping for this code doesn't changes from process to process.
![[Pasted image 20210414110320.png]]

![[Pasted image 20210414110439.png]]

![[Pasted image 20210414110508.png]]

currently stuck at page frame allocator problem this problem described in this [link](https://www.reddit.com/r/osdev/comments/2wot0n/looking_for_some_help_implementing_paging/)
### Issues while writing paging
1. Locate things where-ever we want in memory (Simply map it to the correct logical address and bam, it's now located at that position) 
2. We can easily get contiguous pieces of memory by simply grabbing any random pages we want and mapping them into contiguous logical addresses.
3. A side-effect of paging is that once you turn it on, _everything_ has to use paging, including your kernel. Thus, it's _impossible_ to access a piece of memory through any physical addresses you have, unless you map that physical address into a logical address

## Interrupt Descriptor Table

![[Pasted image 20210407152650.png]]

## PIC
1. manage hardware interrupts and send them to the appropriate system interrupt.
2. Maps hardware interrupts to [[Interrupt Descriptor Table]] [[IDT]].
3. Without a PIC, you would have to poll all the devices in the system to see if they want to do anything (signal an event), but with a PIC, your system can run along nicely until such time that a device wants to signal an event, which means you don't waste time going to the devices, you let the devices come to you when they are ready.
4. Part of the [[Exec page in kernel]]'s job is to either handle these [[IRQ]]s and perform the necessary procedures (poll the keyboard for the scan-code) or alert a user-space program to the interrupt (send a message to the keyboard driver).


## Open Questions
1. How is Segmentation helps in memory protection.
2. How does GDT and LDT ties to Process memory.
3. How is multi-process information stored and managed. 


## Work Logs
1. Got to learn how to interact with the raw device
2. initializing the system
3. assembly and C interactions
4. port interactive or communicating with serial device



## Reference
1. wiki.osdev.org
2. littleosbook.github.io
3. os.phil-opp.com
4. github.com/farizluqman/little-os