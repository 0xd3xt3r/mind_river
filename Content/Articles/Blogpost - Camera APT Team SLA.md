---
up: "[[Writing MoC]]"
tags:
  - "#type/writing/article"
status: todo
created-date: 2025-05-01
---

- What will we do moving forward
	- Results expectation
		- Fuzzing will not yield bugs consistently! It a guided randomness. If you are not finding bugs that mean there is some maturity in the software and if you are finding bugs means we have caught it before an attacker. So it will be a win-win situation.
	- We will periodically run the Syzkaller fuzzer on latest build and Syzkaller and share the results of execution periodically. Also notify if there is drop in coverage.
	- It is important to run the camera in isolation
	- There are two component of the engagement.
		- I will help you to understand "How is this a security problem?". Since I have a expertise in the area and your team will help me to regularly deploy and the fuzzer.(Understand the )
	- Deployment
		- build should have KASAN + KCOV support.
		- Check it with (sanitizer-status)
- What have we achieved so far?
	- Deployed Syzkaller on Kaannapalli code base and found 3 crashes and we have  coverage of 11,100.
