---
up: "[[Vibe Project MoC]]"
tags:
  - type/vibe-specs
created-date: 2026-01-30
related:
  - "[[Claude Code]]"
  - "[[Exprience of Raw Vibe Coding]]"
summary: All the Talks related to AI and LLM
aliases:
  - AI Talk
---

### Solving Hard Problems in Complex Codebases

**Author** – Dex Horthy, HumanLayer
[Link](https://www.youtube.com/watch?v=rmvDxxNubIg)

### Context Engineering
**Author** – Dex Horthy, HumanLayer
[Link](https://www.youtube.com/watch?v=8kMaTybvDUw)

### Building C compiler

[Link](https://www.anthropic.com/engineering/building-c-compiler)

- The scaffolding runs Claude in a loop, but that loop is only useful if Claude can tell how to make progress. Most of my effort went into designing the environment around Claude - the tests, the environment, the feedback-so that it could orient itself without me.
- Write extremely high-quality tests - Claude will work autonomously to solve whatever problem I give it. So it’s important that the task verifier is nearly perfect, otherwise Claude will solve the wrong problem. Improving the testing harness required finding high-quality compiler test suites, writing verifiers and build scripts for open-source software packages, and watching for mistakes Claude was making, then designing new tests as I identified those failure modes.
- Near the end of the project, Claude started to frequently break existing functionality each time it implemented a new feature. To address this, I built a continuous integration pipeline and implemented stricter enforcement that allowed Claude to better test its work so that new commits can’t break existing code.
- I had to constantly remind myself that I was writing this test harness for Claude and not for myself, which meant rethinking many of my assumptions about how tests should communicate results.

For example, each agent is dropped into a fresh container with no context and will spend significant time orienting itself, especially on large projects. Before we even reach the tests, to help Claude help itself, I included instructions to maintain extensive READMEs and progress files that should be updated frequently with the current status.

### Month of Agents : Blog

[link](https://crawshaw.io/blog/eight-more-months-of-agents)


### Short Case of Nvidia
[link](https://jeffreyemanuel.com/writing/the_short_case_for_nvda)

- https://jeffreyemanuel.com/
	- pretty good ideas on how to use agentic coding