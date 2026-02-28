---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/knowledge"
  - type/vibe-specs
status: todo
created-date: 2026-02-08
related:
  - "[[Claude Code]]"
  - "[[LLM Talks]]"
summary: The stream of thougts I had when learning Vibe coding
aliases:
  - Vibe Coding
---

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
10. Where do LLM fits in the whole SDLC?
	1. Any software is basically layer of different components which together makes it a success. The more inner layer which is the core of the a product is very important and which is where you want to spend major or the time. Improving or refining it. 
	2. LLM can help you drastically reduce the time you spend building on outer layer and instead spend more of you time in research and ideation of product and improving the core of your product. Because that's where the MOAT is!
	3. Writing specifications, defining success criteria.
	4. How can you create agents of create other task. digital interns-agents.
11. How do we make output of workflow consumable?

### Mental Models - Deterministic vs Probabilistic outcome

1. Model output can change based on the model you are working with, this kind of outcome might not be a good idea if decision are executed without a review. Such model outcomes may go through and additional reviews. We can say that such scenarios are Probabilistic is nature they depend on the model and how it thinks.
2. While on the other end programs are deterministic in nature the create same output for same set of input every single time no matter which environment you execute them in.
3. The best way to exploit this would be to use hybrid approach. Decompose workflow in different "work units" see if you can convert some of these task can be handled by a program and if not can convert the work in very strict instruction workflow which has additional validation.
4. What is the work you can out-source to LLM and what can be programmed. This will improve as you work more with LLM's and get good intuition of what can be out-sourced.

### Programming Model for Programming Agents

- Prompts are new code. The language is natural.
- Agents are like threads in programming paradigm, they have their own context and objective, they can sync with other threads.
- Skill are modules code, like classes or functions
- slash command - command line tools

### Test Driven Development

When developing the system, what is the problem? That is when you are developing something, the AI will start writing code. And then it will go ahead and add in some other section, which was not intended. This leads to a problem. Intending to create a feature in one section, but the older functionality breaks up. The way to fix this problem is that: You can write test cases. And you can tell the AI to run these test cases every time. A new code is added. This will ensure the integrity of the features that were previously developed. 
	1. This is classically known as Test-Driven Development.
	2. This can also occur in cases where our specifications are not clear. Or the developer is not aware. The other part of the system getting affected because of the new requirement.

### You still need to be a good engineer

AI-assisted coding doesn't eliminate the need for strong Software Engineering skills - it makes it more essential.  
  
Think of it this way: a poor driver is dangerous at 50 km/h in a budget car. But if given access to Lamborghini capable of 400 km/h, the risk doesn't disappear - it amplifies.  
  
Powerful tools accelerate outcomes. Without Solid Engineering - Architecture, Testing, Code-Reviews, Security - the acceleration just gets you to the wrong place faster.  
  
Master the fundamentals. Let the tools amplify excellence, not mistakes.

#SoftwareEngineering #VibeCoding #LLM

### Infinite Software crisis

1. what happens when you can create software at the speed of thoughts
	1. merge queue
	2. code rewriting
2. Who wants all the code?

