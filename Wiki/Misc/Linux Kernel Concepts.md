---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/os/linux"
status:
  - todo
created-date: 2024-12-15
related: 
summary: Linux Kernel Development related stuff
---

# Linux Kernel Development

## Random Notes

- often when two or more task will be operating on a single resource the they will need some kind of sync method. 

## Basic

**container_of** Macro

```c
#define container_of(ptr, type, member) ({               \ 
   const typeof(((type *)0)->member) * __mptr = (ptr);   \ 
   (type *)((char *)__mptr - offsetof(type, member)); })
```


```c
container_of(pointer, container_type, container_field); 
```


```c
struct numbers {
    int one;
    int two;
    int three;
} n;

int *ptr = &n.two;
struct numbers *n_ptr;
n_ptr = container_of(ptr, struct numbers, two);
```

You have a pointer that points in the middle of a structure (and you know that is a pointer to the filed `two` [_the field name in the structure_]), but you want to retrieve the entire structure (`numbers`). So, you calculate the offset of the filed `two` in the structure:

`offsetof(type,member)`

and subtract this offset from the given pointer. The result is the pointer to the start of the structure. Finally, you cast this pointer to the structure type to have a valid variable.

## In-Build Data Structures
Data structure which improve the developer productive for common task

1. Link List
2. Queue
3. Trees

## Processes/Task

## Process Scheduling

## Interrupt Processing

## Deferred Work

### Top Half

This processing function which immediately get triggered when an interrupt is triggered. Does the minimum processing required and puts the work in later execution, which is usually done in Bottom Half

### Bottom Half

### Tasklets

#### Softirqs

#### Work Queues 

## Kernel Synchronization Mechanism

###  Locks
This mechanism is usually used to prevent parallel read and writes to share data structures which could create Race-condition or in-correct program behavior.

#### Spin Lock

Acquire the locks

#### Semaphores

#### Mutex

#### Conditional Variables

Setting/unsetting these variables trigger another part of the code in the kernel.

#### Count Locks

## Memory Management
This sub-system deals with allocating and managing memory required by kernel.

**Memory is divided into three zones :**
1. Direct Memory Access (DMA)
2. Normal Zone - memory less then 896 MB
	1. A portion of the kernel virtual address space can be mapped as a single contiguous chunk into the so-called physical "low memory".
3. High Memory Zone - memory higher then 896 MB.
	1. "High memory" defined as the portion of **PHYSICAL** memory that cannot be mapped contiguously with the rest of the kernel virtual memory.
	2. has no logical address
	3. not physically contagious when used in kernel
	4. only in 32 bit
- "High Memory" is used for application space and "Low Memory" for the kernel.

![[Pasted image 20210911135027.png]]
![[Pasted image 20210911135042.png]]
- Linux used slab allocation when you make allocation request for byte size granularity.

There are three type of allocator
- SLAB - allocator focus on hardware cache optimization.
- SLUB - new standard allocator since 2007, use in Ubuntu, CentOS and Android.
- SLOB - designed for embedded systems.

### kmalloc

1. allocate memory which is physically contiguous. 


### vmalloc
1. allocates memory which is virtually contiguous and not necessarily physically contiguous.

**Copy memory to and from User-space**

There is a function *copy\_to\_user()* and *copy\_from\_user()* provided my kernel, its best to use this function as it has bunch of security features.
1. Integer overflow protection
2. Kernel page tables check is done to make sure user-space and kernel space page tables are not interchanged during data copy.
3. These functions also check if the page from/to which operating on is mapped if its not mapped then it maps it.

#### References

- https://elinux.org/images/b/b0/Introduction_to_Memory_Management_in_Linux.pdf
- https://hammertux.github.io/slab-allocator

### Allocation Flag

### Kernel Stack

## Virtual File System

## Block IO Sub-System

The newer version of the Block IO system is focus around `bio` struct.

## Process Memory

1. The process memory called address space in kernel terminology is represented by `mm_struct`data structure. 
2. If multiple process share the same memory area then they point to the same [[mm_struct]], which is usually the case for threads.
3. Not all the process memory is accessible. It is broken down by different section and each section have different permission(like read, write and execute) which is represented by [[vm_areas_struct]].


## Bluetooth Stack
Linux uses [[BlueZ]] as its [[bluetooth]] protocol stack.

![BlueZ Protocol Stack](https://i.stack.imgur.com/wsFHr.png)