---
tags:
  - type/writing/social-media
status: published
created-date: 2024-10-12
---
## Post #1 - Debugger Part 1

This is a follow up post on "Internal working of Debuggers?" 💡  
  
We already seen that the process which has to be debugged is lunch such that another process can observer control it, the question that arises how is all this thing coordinated between Tracee and the Debugger.  
  
To facilitate debugging, Kernel acts as a the mediator between the Debugger and the Debuggeee. Since it has more privilege than normal process, and it controls the execution of the processes. Since it already know a lot about any process and control in it many ways, it may as well can pass on this information to the Debugger in the form of event.  
  
These events as what facilitates the debugging process, events could be :  
1. System Calls like Reading/Writing Files  
2. Sending data over network or Communicating with Device Drivers  
3. Signals received by the process like Stop, Kill, or Segmentation Faults.  
4. Or anything that could invoke the Kernel.  
  
One particular event is which extensively used during debugging is Breakpoint event. When you put a Breakpoint to inspect a particular line of code, what debugger internally does is that, it replaces that line of code with a Breakpoint Instruction. While executing the code whenever the process encounters this Instruction it generates a exception the process which is then catch by the Kernel, it stops the process and generate the Breakpoint event to the Debugger process.  
  
🔗 You can find the details of these event in ptrace docs Link [https://lnkd.in/gdmahrBX](https://lnkd.in/gdmahrBX)  
  
As a Security Engineer harnessing the power of debuggers can be very powerful, it can help you trace execution, events even if you don't have a source code. This is how Hacker create game cheat code and cracks licenced software.  
  
🚨 🚨 🚨  
Food for thoughts  
  
If Kernel is helping you to debug your user-land process, how will you debug the Kernel code? 🤔  
  
Who will mediate the mediator? 🤠  
  
⚡ ⚡ ⚡
  
I am passionate about System Security and I write on topics related to it on my blog [https://taintedbits.com](https://taintedbits.com/)