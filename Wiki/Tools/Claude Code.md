---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/knowledge"
  - "#type/pinned-docs"
status: in-progress
created-date: 2026-01-11
related: "[[LLM Talks]]"
summary: how to use claude code in best possible way
aliases:
  - claude-code
---

## Notes

[[Exprience of Raw Vibe Coding]]

---
# Components of the framework

What are different components and how are they useful and what purpose do they server?

## Context Engineering

> Context engineering is the delicate art and science of filling the context window with just the right information for the next step.
> Art because of the guiding intuition around LLM psychology of people spirits

This helps the LLM to have relevant information before starting the task.
- This is the most important component which will remarkably improve your performance of the tool.
- There are different components which the framework provides to minimize the context relevant to the you task.

- **Challenges**
	- How to you compress context after a long conversation into only relevant information.
	- This problem is called as context management
	- How to you store and retrieve the conversation using external memory?
		- There are some task management systems like [beads](https://github.com/steveyegge/beads/)
		- Sequential thinking plugin part of MCP plugins
	- Interesting thing would be to create a high level requirement but as more information is discovered, task are dynamically added to the task list and executed and the list can keep growing and shrinking as the work progress.
	- I had a thought, that current models are limited by the number of tokens they can process, that's true. But ain't we also as human being working on limited set of context window! 
		1. While doing programming task, its not like we are digest entire codebase before we start working on new feature which we intend to add to the codebase? 
		2. No, we start by understand the area of code base where we want to make the changes. We try to understand it function what does different variable, function or class means, if the complexity of the code is high we make assumptions and try to complete our work.
		3. then we make the change.
		4. We don't stop there, then we go ahead to test those changes, manually or automatically(unit-test, integration test) but we test and validate our understand and if the changes produce undesirable results, 
		5. We go back to the step one or validating our understanding or the assumption we have made or by means of debugging, re-read the code or consulting a college.

## Skill

> This help the claude to call upon a specific skill it need to complete a task, something like pdf parsing.

A Skill is a set of instructions and/or code that Claude can run over and over again, on demand. They're kinda like custom slash commands, except more powerful.

Let's say you want to update your team via Slack every time you push a new feature. You may have a Slack MCP or a custom script set up already but then you'd have to tell Claude to run this process every single time.

Instead, now you can just package up the instructions into a Skill. From the Skill metadata, Claude knows to use that skill when you're pushing code, and will automatically do that.

What makes it better than MCP is that it doesn’t eat up your context. MCP always loads up front when you start a session, so it uses up your context window unnecessarily.

## Command

1. Also called as slash command
2. This is very similar to skill but the difference is you can use this to call multiple skill to accomplish a particular task.
3. Skill is invoked by claude while the command is invoked by the user

## Agent

## MCP

> Connecting AI assistants to external tools and data sources

You can think of this as body organs coordinating with your brain, LLM's are the brain by to carry out the functionality you need to interact with the external world which is exactly what MCP does. It can execute thing in the you computer by the interface which you have exposed.

## Skills vs Slash Commands 

> (Claude Code Only)

If you’re a Claude Code user, you may have come across custom slash commands that allow you to define a certain process and then call that whenever you need.

This is actually the closest existing Claude feature to a Skill. The main difference is that you, the user, triggers the custom slash command when you want it, and Skills can be called by Claude when it determines it needs that.

Skills also allow for more complexity whereas custom slash commands are for simpler tasks that you repeat often (like running a code review).

---

# Internal Tools

> Tool created in Qualcomm

1. [AI Root Cause Analysis](https://github.qualcomm.com/QPFI/AI_Root_Cause_Analyzer/)
2. [Stateful engine](https://github.qualcomm.com/QPFI/Stateful_Fuzzing_Engine/)
3. [Using Qgeine with claude-code](https://github.qualcomm.com/ncouffin/qgenie_proxy)

## Official Docs

1. [Getting started](https://www.anthropic.com/engineering/claude-code-best-practices)
2. How to write skill
	1. [Claude Docs](https://code.claude.com/docs/en/skills)
	2. [how to write better skills](https://sidbharath.com/blog/claude-skills/)

## External Tools

1. Sequential Thinking - Solving complex problems with this workflow
	1. [mcp](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking)
	2. [workflow](https://github.com/cline/prompts/blob/main/.clinerules/sequential-thinking.md)
2. [Cline](https://github.com/cline/cline)
	1. Advance agent for vscode
3. [12 Factor Agent](https://github.com/humanlayer/12-factor-agents/)
4. [Complex Task with Agents](https://github.com/humanlayer/humanlayer/)

## Reference

1. [Claude code repo](https://github.com/anthropics/claude-code)
2. [Vibe engineering](https://simonwillison.net/2025/Oct/7/vibe-engineering/)
3. [Claude skill library](https://skillsmp.com/)
4. [Agentic Coding](https://simonwillison.net/2025/Oct/5/parallel-coding-agents/)
