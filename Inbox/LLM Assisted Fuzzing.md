---
up: "[[Research Paper MoC]]"
tags:
  - "#reading/research-paper"
status: todo
created-date: 2025-10-16
summary: Using LLM to improve the fuzzing harness
---

## Notes

1. Team Atlanta has published 150 page paper on their work. They were the winner of the AIxCC competition.
	1. [Paper Link](https://arxiv.org/pdf/2509.14589)
	2. Type of Challenge
		1. Full-code challenge - upload full-code to find the vulnerability
		2. Delta-scan challenge - use new patch code to find vulnerability
	3. Terms
		1. PoV - Proof-of-Vulnerability
		2. CP - challenge project
	4. budget constraints, strategically allocating LLM API usage and Azure compute resources to maximize their vulnerability
	5. Correct SARIF reports describe harness-reachable, sanitizer-triggered crashes.
	6. SARIF reports can be considered as static analysis results
	7. They have used custom fine-tuned LLM.
	8. For delta-mode CPs, CP-MANAGER additionally verifies that the PoV does not crash on the BASE version, ensuring the vulnerability exists only in the target changes.

## Core Idea


## Connected Concepts
