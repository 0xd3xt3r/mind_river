---
up: "[[Learning MoC]]"
tags:
  - Windows
  - migitation
  - vulnerability-research
  - "#type/video-stream"
media_link: https://www.youtube.com/watch?v=A-vtsqhXcro
status: done
created-date: 06-12-2024
---

## Annotations

1. [12:12]() - What is CFG, based on CFI.
	1. It tires to mitigate UAF
2. [14:56]() - CFG is be bypassed with stack overwrite & ROP Shellcode.
	1. its mitigated with [[CET]] (Control Flow Enforcement Technology) protect using shadow stack.
3. [16:13]() - performance impact, tested on Chakra Core
4. [18:27]() - Reversing CFG
5. [20:21]() - compile with CFG function pointer is replaced with \_\_guard_dispacth_icall_fptr
6. [21:37]() - CFG Flow diagram (Windows Internals Part 1 - 7th edition)
7. [24:31]() - function disassembly
8. [26:40]() - Discussion about CVE-2024-22058 (no CFG Bypass)
9. [28:48]() - target reversing
10. [33:31]() - resumed a crash
11. [36:34]() - target vulnerability
12. [38:14]() - CVE Details on the website
13. [41:19]() - Backward compatibility
14. [46:44]() - CVE Demo
15. [48:10]() - rp++ rop gadget
16. [48:54]() - Discussing CVE-2019-0567
17. [51:24]() - debug the exploit
18. [52:55]() - security failure
19. [53:36]() - CFG bypass exploit demo - stack pivot address should be within 16 bytes range of valid function call. use rp++ to find the gadgets.
20. [54:00]() - CFG Mis-conceptions 1 - protect forward edge, doesn't protect the return edge.
21. [54:29]() - CFG Mis-conceptions 2 - only first ROP Gadget has to be CFG compatible other ROP jump can do anything.
22. [55:52]() - rp++ gadget finding results
23. [56:30]() - CFG compatible gadgets
	1. valid function start address but they are uncommon.
	2. within 16 bytes boundary range.
24. [57:24]() - CFG gadget finding challenges
26. [58:17]() - actual exploitation strategy 
27. [58:44]() - exploit strategy by Sims
28. [59:04]() - why stack pivot in case of [[heap exploit]], [[type confusion]] or [[UAF]]
29. [59:49]() - Microsoft CFG mitigation bypass bounty program
30. [1:00:36]() - Bypass technique 1
31. [1:03:46]() - LdrpDispatchUserCallTarget function and CFG internals
32. [1:07:31]() - CFT advantages
33. [1:08:10]() - Bypass technique 2
34. [1:13:43]() - kernel CFG
	1. kernel bitmap is protected VBD(virtualization based security) permissions
35. 


## Reference Notes

1. Side talk
	1. Counterfeit Object-Oriented Programming Vulnerabilities 
		1. [[Counterfeit Object-Oriented Programming]] (COOP) is a code reuse attack that allows hackers to hijack objects in a program to control its execution flow. COOP exploits can be difficult to mitigate because they can bypass defenses that don't consider C++ semantics. Here are some ways to mitigate. [31:00]()
		2. Bypassing Intel CET with Counterfeit Objects.
	2. [31:33]() - [[Shadow Stack]]
2. [[CFG]] - Control Flow Guard. It tries to mitigate vulnerabilities which rely on function pointer overwriting like UAF.
	1. [[CET]]
	2. its only protects forward edge
	3. Return Flow Guard - an old mitigation my microsoft technology
	4. shipped with windows 8.1
3. Function pointer call is replaced with function call which does the verification of the destination call address.
4. Injected instrumentation code is a function call to (LdrpDispatchUserCallTarget) which check if the address is valid (an intended jump) or a malicious divergent. This check is an array lookup for the destination address. The is array is actually a bitmap index with stores 2 bits for each address
5. Address metadata is stored in bitmap, address is 2 bits
	1. valid address
	2. valid address but suppressed - should not be called in-directly like VirtualAlloc. Blacklisted function, or jump into belly of the function.
	3. address is within 16 bytes range of function address is still valid jump ( one can use this for search for valid ROP Gadgets)
6. For each call address there are two bit information stored in a memory which describes the destination address.
7. The function call 
8. [[Mitigation Bypass]]
	1. doesn't protect return address, its protected by CET.
	2. **Non-CFG Module** Non-CFG module affect CFG in two aspects.
	3. **JIT Generated Code** JIT generated code is just like a non-CFG module. It also may contain unprotected indirect call, and all its corresponding bits in the CFG Bitmap are set. 
9. BYPASS CONTROL FLOW GUARD COMPREHENSIVELY - Blackhat paper
10. [rp++](https://github.com/0vercl0k/rp) - fast rop gadget finding tool
11. [Windows Docs](https://learn.microsoft.com/en-us/windows/win32/secbp/control-flow-guard) - how to enable and how to check if you have the mitigation.
	1. You can enable this by /guard:cf option in the compiler.


## Permanent Notes

CFG Stands for Control Flow Guard. it is a static instrumentation technique where a compiler injects additional code to do some additional check before passing on the control to the destination function.

## Task

- [x] Complete watching "Windows CFG Mitigation" stream #learning #task ➕ 2024-12-06



