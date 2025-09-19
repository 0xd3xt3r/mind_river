---
tags:
  - type/writing/social-media
  - compilers
status: todo
created-date: 2024-12-05
---

## Post - Struct padding and memory disclosure bugs
- Padding & Re-ordering
	- This is done to improve the CPU access speed.
- Re-ordering
	- The C++ standard guarantees that the members of a class or struct appear in memory in the same order as they are declared.
	- doing this can actually make the struct packing more efficient but it won't do that
- https://www.nccgroup.com/us/research-blog/padding-the-struct-how-a-compiler-optimization-can-disclose-stack-memory/
- https://jonasdevlieghere.com/post/order-your-members/