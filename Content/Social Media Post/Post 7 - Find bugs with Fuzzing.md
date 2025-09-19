---
tags:
  - type/writing/social-media
status: published
created-date: 10-12-2024
---
## Post #7 - Find bugs with Fuzzing

⚡Software Exploitation Beyond Stack overflow - Part 2⚡

Essential aspect of Software Exploitation is discovering new vulnerabilities. When your get into the problem of finding new bugs there are several Strategies like Static Analysis, Fuzzing and Code Auditing. I tend to use these three strategies in my day-to-day work.

Although it may seem like three different strategies/tooling but they are not! they actually compliment each other. But before talking about how we can combine them, lets first try to understand them individually.

Static Analysis is basically trying to find vulnerabilities by pattern matching. Vulnerability pattern are expressed in a query language provide by the Static Analysis tool. These tools tend to create internal model of the target code base and based on the query it match that model just like SQL query matches the data stored in Database.

When programmers write code they tend to recognize some common problems of the application and solve them with common solution(like creating a linked-list library), this idea tend to evolve in to something call as Design Pattern. Just like a good design pattern can solve a problem, a bad pattern can lead you into one. Yes! a Software Vulnerability.

Next in line is Fuzzing, in this method you feed random inputs to your target with user-controlled data, observer for any crashes. You collect all the inputs that caused the crash and analyze it for security issues. Important thing note here is that this is a dynamic approach, where you are executing the code unlike Static Analysis.

So if you are dealing with Linux or Windows based target then there numerous fuzzing tools like AFL++, WinAFL, Hongfuzz and what-not. But if you dealing with Embedded Systems and you need to figure-out a way to trigger the target code on device and observer crashes and restart the target. You need to keep doing that automatically to execute as many test case as possible.

While above two technique are Static and Dynamic approach they both need a very crucial input and that is understand of the Target Application. Different targets are different software pattern and different set of challenges. Here is a rough category of targets:

1. Network Service like Web Server, Game Server, RDP Server/Client, etc.
2. Media Codec/File Format parser, for eg JPEG, MP4, PDF, Docx
3. Operating System Kernels like Linux, Windows, XNU, RTOS, etc.
4. Web Browser like Rendering Engine, DOM Parser, Javascript Engine, etc.

This understanding is developed from Code Auditing. 

Understand the Use-cases, Entry Points, Data Flow, UML, Trust boundaries of the software this will help you create the Threat Modelling. Identify the Asserts of the system and list down the threats.

Based on this understanding you can write the rules for common mistakes your team is making. You can also feed this understanding to the fuzzing harness to reach deeper part of the code.

🍋 🍉 🍇 (c'mon its summer)