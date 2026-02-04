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

1. Break down large body of complex task in to smaller, more approachable units of work. Now, these smaller task we can be either accomplish by script(which give more deterministic output) or LLM(which can be a probabilistic output).
2. We have to create a workflow in terms of "unit of work". A particular unit is doing very specific well define work, in which you give specific input and output defined. This "unit of work" can be converted into SKILL which the agent can deploy to execute.
3. LLM Workflow - Explore, plan, code, validate.
4. The new way of programming has shifted from learning a syntax of language to writing instruction in English. This is just like instructing a dump junior engineer.
5. I have lost the fun of create and debug the program. The way I look at the program has changed completely, I spending more time reading code and its documentation. The I organize the program has also changed. The moat now is in new and novel idea, creating program will be come cheap.
6. A good mental model about vibe-coding is that you are working with a Junior engineer and you give it a very specific instruction. A voice interface can improve the quality of experience.
7. The programming slit replay of algorithmic model, you have series of steps to execute stitch together of input-output. But instead of programming language, now it just natural language.
8. Help the LLM's to navigate the code just how you will navigate the code, only few variable and function and code paths at a time. You can only hold so much amount of information, how can you use this limitation to get thing done. Now both, you and LLM standing with same set of limitation but how can you instruct it with right amount of information to execute what you want? Use this question to craft your context. Context Engineering, they say!
9. Vibe Coding
	1. Documentation - Documentation is now include everything like how to setup environment, directory structure, architecture, etc.
		1. Documenting how you program work can help you to minimize text required the create a proper context. Since you are specifying the interface input-output, or how to use the program and get things done instead of LLM reading code to understand what the program does.
		2. Docs can be done by writing text description on what the program does and when to use it.
		3. Give sample examples on how to use the script, like sample command and what does the output mean much like '--help' command output.
		4. The consumer of this documentation is not going to be other engineers but also LLM Agents who will be creating more code, write test or generating other supporting documents. It become more important to do it and do it right!
	2. Unit test cases - this help you validate the program which is written and when the LLM starting making any changes you can revert it back or debug it with the test cases.
		1. Trusting LLM blindly is not a good strategy to use it, test case are one example of feedback which it can use it to debug.
		2. LLM can produce a perfectly good looking code which is completely again what you asked it to do. Test driven development is good way to create binding contract on what need to be done and test it as and when your project evolves.
	3. Dependency management - This is can be a quality of life improvement, because its possible and i have happened to me once that, You have written a piece of code which works well in your dev environment, but when you take to other environment and tell the LLM to run the code and it encounters the error and to fix it goes in to rewrite-debug loops and comes up the new solution. Once possible reason for this is the package version are different your dev environment as a certain version which the different from the new environment, may be some API's have change or new warning about API deprecation has started to pop up and LLM thinks it a good idea to fix it right now. Best way to avoid this is to lock the deps in virtual env or docker and let it LLM's work in such locked environment. Time and again try to run your program in new environments.
	4. Version control - Plan you work with high level objective, but to progress you have to break it down into small manageable chunks you have created with with back and forth with LLM's has to checked-in this is important because LLM might make check to portion of check which you didn't intend to or you don't agree to what LLM have produced you would want to revert the change back and give it a fresh prompt which is much better than your previous attempt.
		1. You can revisit the changes for review that, just how it was meant to be used.
		2. It would be a good idea to let the LLM generate the summary of the commits and embed a small signature to show that the commit was made by LLM.
	5. Much of what was discussed so far is already part of good engineering practices. But these practices can become a force multiplier; they help you to create better context engineering. The result of these good practices are not just consumed by other engineer's for ramp-up but also by LLM which are helping you to write more code. If you do this well you will treated with better code generation and less mistake.
	6. Generating supportive scripts : like installing dependencies, runtime environment and test automations. These small help-scripts minimize your token limit consumption have deterministic out-come of you request.
	7. Code Review - you will be reviewing code and docs all the time, because by virtue of using LLM's that what you asked for when you have someone else writing the code, the one who you can't fully trust, that what you have to do, you have to review their work. You have to validate this output but that ok! because you have saved yourself lots and lots of keystroke you may as well use that time somewhere more useful.
		1. This is a good opportunity to completely rethink our code editor, as part of my work I was spending more time reviewing the diff code that it made me think, i should be rather look have a code editor which has good git integration.
	8. Deterministic vs Probabilistic outcome - 
		1. Model output can change based on the model you are working with, this kind of outcome might not be a good idea if decision are executed without a review. Such model outcomes may go through and additional reviews. We can say that such scenarios are Probabilistic is nature they depend on the model and how it thinks.
		2. While on the other end programs are deterministic in nature the create same output for same set of input every single time no matter which environment you execute them in.
		3. The best way to exploit this would be to use hybrid approach. Decompose workflow in different "work units" see if you can convert some of these task can be handled by a program and if not can convert the work in very strict instruction workflow which has additional validation.
		4. What is the work you can out-source to LLM and what can be programmed. This will improve as you work more with LLM's and get good intuition of what can be out-sourced.
10. Where do LLM fits in the whole SDLC?
	1. Any software is basically layer of different components which together makes it a success. The more inner layer which is the core of the a product is very important and which is where you want to spend major or the time. Improving or refining it. 
	2. LLM can help you drastically reduce the time you spend building on outer layer and instead spend more of you time in research and ideation of product and improving the core of your product. Because that's where the MOAT is!
	3. Writing specifications, defining success criteria.
	4. How can you create agents of create other task. digital interns-agents.

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
