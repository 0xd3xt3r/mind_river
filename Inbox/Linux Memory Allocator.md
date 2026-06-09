---
up: "[[Linux Kernel]]"
tags:
  - "#type/knowledge"
status: todo
created-date: 2026-05-29
related: "[[Linux Kernel]]"
summary:
---

## Page / Buddy Allocator



## SLUB Allocator

### Kernel APIs

The Linux kernel exposes two primary sets of APIs for interacting with the SLUB allocator.

You can think of these in two categories: **Dedicated Caches** (creating your own custom "retail store" for a specific object structure) and **General Purpose Allocators** (using pre-existing, generic sizes).

Here are the core APIs and how they are used.

---

#### 1. Dedicated Cache APIs (`kmem_cache_*`)

When a kernel subsystem (like a filesystem or networking stack) frequently allocates and frees a specific data structure (e.g., `struct inode` or `struct sk_buff`), it creates a _dedicated cache_. As a security researcher, these are crucial because isolating objects in dedicated caches prevents them from merging with generic caches, reducing the risk of cross-cache exploitation.

**Creation & Destruction**

- `kmem_cache_create(const char *name, unsigned int size, unsigned int align, slab_flags_t flags, void (*ctor)(void *))`
    - **Purpose:** Initializes a new cache.
    - **Security Note:** The `flags` parameter can include `SLAB_ACCOUNT` (which isolates memory to a specific cgroup) or `SLAB_HWCACHE_ALIGN` (aligns objects to L1 cache lines, manipulating the heap layout). The `ctor` (constructor) initializes an object when the underlying page is first carved up, _not_ every time the object is allocated.
- `kmem_cache_create_usercopy(...)`
    - **Purpose:** A hardened version of cache creation.
    - **Security Note:** If an object needs to be copied to or from user space (via `copy_to_user`), this API restricts _exactly which region_ (offset and size) of the object is allowed to be copied. This is a massive mitigation against kernel information leaks and arbitrary memory overwrites.
- `kmem_cache_destroy(struct kmem_cache *s)`
    - **Purpose:** Destroys the cache and releases all underlying slabs back to the Buddy Allocator.

**Allocation & Freeing**

- `kmem_cache_alloc(struct kmem_cache *s, gfp_t flags)`
    - **Purpose:** Fetches a single object from the specified cache. The `gfp_t` (Get Free Page) flags tell the allocator how hard it should try to find memory (e.g., `GFP_KERNEL` allows sleeping, `GFP_ATOMIC` prevents sleeping and is used in interrupt contexts).
- `kmem_cache_free(struct kmem_cache *s, void *p)`
    - **Purpose:** Returns the object `p` to the freelist of cache `s`.

---

#### 2. General Purpose APIs (`kmalloc` family)

If a kernel developer just needs a quick buffer (like a 64-byte array for a temporary string), they don't create a dedicated cache. Instead, they use the `kmalloc` family.

Under the hood, the kernel boots up with a series of generic, power-of-two SLUB caches (e.g., `kmalloc-8`, `kmalloc-16`, `kmalloc-32`, `kmalloc-64`... up to `kmalloc-8192`).

- `kmalloc(size_t size, gfp_t flags)`
    - **Purpose:** Allocates a block of memory. SLUB automatically rounds the `size` up to the nearest power-of-two cache. (e.g., requesting 40 bytes will pull from the `kmalloc-64` cache).
    - **Exploitation angle:** This rounding behavior causes _internal fragmentation_, creating "padding" at the end of objects. Attackers can sometimes use this padding to safely overflow an object without crashing the kernel.
- `kzalloc(size_t size, gfp_t flags)`
    - **Purpose:** Exactly like `kmalloc`, but it passes the `__GFP_ZERO` flag, ensuring the allocated memory is zeroed out before being returned. This breaks uninitialized memory leak exploits.
- `kcalloc(size_t n, size_t size, gfp_t flags)` / `kmalloc_array()`
    - **Purpose:** Used to allocate arrays safely. These include internal checks (`check_mul_overflow`) to prevent integer overflow vulnerabilities when multiplying `n * size`.
- `kfree(const void *p)`
    - **Purpose:** Frees the generic memory. It automatically determines which `kmalloc-X` cache the object belongs to and returns it to that specific freelist.

---

### Summary

The SLUB allocator's API surface is designed around `kmem_cache_*` for high-frequency, strictly-typed objects and `kmalloc` for generic, arbitrary-sized allocations. From a vulnerability research perspective, the line between these two API sets is where modern mitigations (like hardened usercopy and cache isolation) are enforced to break predictable heap layouts.

### Internal structure

| structure       | Description                                                                              |
| --------------- | ---------------------------------------------------------------------------------------- |
| kmem_cache      | contains all the information needed for slab cache management                            |
| kmem_cache_cpu  | object holds per-CPU information for a slab cache                                        |
| kmem_cache_node | Partial slabs are tracked centrally per NUMA node in a structure called kmem_cache_node. | 

1. All structs are in this file [include/linux/slub_def.h](https://elixir.bootlin.com/linux/v5.19.17/source/include/linux/slub_def.h#L90)

![[slub_allocator_data_struct_relation.png]]

```C
struct kmem_cache_cpu {
	void **freelist;	/* Pointer to next available object */
	unsigned long tid;	/* Globally unique transaction id */
	struct slab *slab;	/* The slab from which we are allocating */
#ifdef CONFIG_SLUB_CPU_PARTIAL
	struct slab *partial;	/* Partially allocated frozen slabs */
#endif
	local_lock_t lock;	/* Protects the fields above */
#ifdef CONFIG_SLUB_STATS
	unsigned stat[NR_SLUB_STAT_ITEMS];
#endif
};


/*
 * Slab cache management.
 */
struct kmem_cache {
	struct kmem_cache_cpu __percpu *cpu_slab;
	/* Used for retrieving partial slabs, etc. */
	slab_flags_t flags;
	unsigned long min_partial;
	unsigned int size;	/* The size of an object including metadata */
	unsigned int object_size;/* The size of an object without metadata */
	struct reciprocal_value reciprocal_size;
	unsigned int offset;	/* Free pointer offset */
#ifdef CONFIG_SLUB_CPU_PARTIAL
	/* Number of per cpu partial objects to keep around */
	unsigned int cpu_partial;
	/* Number of per cpu partial slabs to keep around */
	unsigned int cpu_partial_slabs;
#endif
	struct kmem_cache_order_objects oo;

	/* Allocation and freeing of slabs */
	struct kmem_cache_order_objects min;
	gfp_t allocflags;	/* gfp flags to use on each alloc */
	int refcount;		/* Refcount for slab cache destroy */
	void (*ctor)(void *);
	unsigned int inuse;		/* Offset to metadata */
	unsigned int align;		/* Alignment */
	unsigned int red_left_pad;	/* Left redzone padding size */
	const char *name;	/* Name (only for display!) */
	struct list_head list;	/* List of slab caches */
#ifdef CONFIG_SYSFS
	struct kobject kobj;	/* For sysfs */
#endif
#ifdef CONFIG_SLAB_FREELIST_HARDENED
	unsigned long random;
#endif

#ifdef CONFIG_SLAB_FREELIST_RANDOM
	unsigned int *random_seq;
#endif

#ifdef CONFIG_KASAN
	struct kasan_cache kasan_info;
#endif

	unsigned int useroffset;	/* Usercopy region offset */
	unsigned int usersize;		/* Usercopy region size */

	struct kmem_cache_node *node[MAX_NUMNODES];
};

/*
 * The slab lists for all objects.
 */
struct kmem_cache_node {
	spinlock_t list_lock;

#ifdef CONFIG_SLAB
	// ignore
#endif

#ifdef CONFIG_SLUB
	unsigned long nr_partial;
	struct list_head partial;
#ifdef CONFIG_SLUB_DEBUG
	atomic_long_t nr_slabs;
	atomic_long_t total_objects;
	struct list_head full;
#endif
#endif

};
```

### Question

1. Why there are two freelist now, kmem_cache_cpu->freelist and kmem_cache_cpu->slab->freelist why two?
	1. we have to look at a very specific concurrency problem: **What happens when CPU 1 frees an object that belongs to CPU 0's active slab?**
	2. How do these two lists ever sync back up? The magic happens when CPU 0 runs out of objects in its fast-path list.
		1. CPU 0 keeps allocating from `kmem_cache_cpu->freelist` until it is completely empty (`NULL`).
		2. CPU 0 thinks, _"My active slab is empty. I need to unfreeze it and go find a new partial slab."_
		3. **The Catch:** Before discarding the active slab, CPU 0 looks at the return bin (`slab->freelist`).
		4. If CPU 1 or CPU 2 returned objects to this slab while CPU 0 was busy, `slab->freelist` will **not** be NULL.
		5. CPU 0 simply takes the entire chain of objects sitting in `slab->freelist`, moves them over to its own `kmem_cache_cpu->freelist`, sets `slab->freelist` to NULL, and keeps using the same active slab!
		6. If `slab->freelist` is also NULL, then the slab is truly empty, and CPU 0 finally unfreezes it and goes to the `kmem_cache_node` to find a new one.
2. when we say we are pinning the *kmem_cache_cpu* to CPU0 this pinning is maintained for entirety of kernel life of for only thought a syscall execution.
3. How is the *kmem_cache_cpu* is pinned to particular CPU? What does this mean by pinning? what happens when the kernel thread is pre-empted by hard IRQ. in-case of real-time OS.

### Notes

1. What are the different data structs/levels of allocation and how does that affect the performance.
2. What are the different 
3. kmalloc which is done with GFP_KERNEL_ACCOUNT flag is place in kmalloc-gc-* slab. This flag is meant to copy data in the special heap which contains allocation trigged by from untrusted user-space calls. This make exploitation difficult because kernel data is stored in kmalloc-* slab. This change was done in kernel 5.17 onwards.


### Ref

1. SLUB Allocator Internal - Oracle Blog
	1. [Part 1](https://blogs.oracle.com/linux/linux-slub-allocator-internals-and-debugging-1)
	2. [Part 2](https://blogs.oracle.com/linux/linux-slub-allocator-internals-and-debugging-2)
	3. [Part 3](https://blogs.oracle.com/linux/linux-slub-allocator-internals-and-debugging-3)
	4. [Part 4](https://blogs.oracle.com/linux/linux-slub-allocator-internals-and-debugging-4)
2. https://www.youtube.com/watch?v=2hYzxsWeNcE
3. https://syst3mfailure.io/linux-page-allocator/