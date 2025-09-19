---
tags:
  - type/writing/social-media
status: published
created-date: 2024-12-11
---
## Post #2 - Debugger Part 2

 Ever wondered about the inner workings of Debuggers? 🕵️‍♂️  
  
Any software development requires you do some sort of debugging. Usually this is done with help of debugging software's like gdb, windbg or some fancy UI debugger integrated in your IDE. But how do debuggers work?  
  
But first, We need to understand what a debugger is capable of? What a Debugger essentially does is halt a running a process at point which developer want. That could be a buggy line of code or function whose parameter you want to observer/change, basically anywhere in a running process. Not only that you can also change the variable values and make you code jump around anywhere in halted process. You can also attach a debugger to a already running process and do all these things.  
  
But, How is it able to do this? Who gives debugger such a power over other running processes? A power to observe, control and manipulate other processes? How evil is that! 🤔  
  
What really happens under the hood is that Operating Systems exposes bunch of API's which allows one process(debugging process) to observe, control and manipulate another process(tracee process). Using these API's you create a debugger, which will tell the operating systems to lunch a process under my (debugger) observation. You will the tell OS using the API's to halt at appropriate location and you can make changes to the running process and continue its execution as-if nothing happened.  
  
For Linux exposes this via ptrace API's and Windows also have something similar. And every OS have some equivalence of debugger API/  
🔗 ptrace Docs Link - [https://lnkd.in/gdmahrBX](https://lnkd.in/gdmahrBX)  
  
As a Security Engineer understanding and harnessing the power of debuggers can be very helpful, it can help you with all the above mentioned things even if you don't have a source code. This is how hacker create game cheat code and software cracks.  
  
🚨 🚨 🚨  
  
Food for thought, if debugger themselves are software, an you need debugger to assist you in software development, then How can you debug a Debugger?