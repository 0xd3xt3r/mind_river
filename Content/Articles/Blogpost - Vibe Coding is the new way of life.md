---
up: "[[Writing MoC]]"
tags:
  - "#type/writing/article"
status: todo
created-date: 2026-01-24
---

For past couple of months LLM's have really improved, I has some mixed results in the past especially regards to code task. But more recently not only the models have improved, but also they way they are integrated with the work environment has made them even more effective. I was very skeptical initially but as I started using it i was impressed but it capabilities.

I would simply write a prompt and the cli-agent would go ahead and produce code which it would test and documentation for the tool. I was very impressed when the code the create it tested and went ahead and did the debugging and test if it didn't work, it would break down the large feature in the small manageable chunks and later would integrated all of that.

Such execution left me thinking, then what left for me to do? Only write good prompts? Sure, very specific and as detail was possible but again felt relived of not worrying about silly syntax errors, and it felt like you thought are directly translated to code just with few words.

In this blog I will explore the mental mode I am employing to get things done, it not about writing prompts, I am sure there are plenty of post out there to do it. But the topic of this post is to the mental model one must apply to get things done more effectively. The tool in particular we are going to discuss is "claude code" but the same technique could be applied else with other open source solutions like "qwen-code" and devstal-code which has more or less the same thing going on. May be this could turn into a new way of programming, just how assembly programming move to higher-level language programming which introduced more sophisticated tool for code reusability and dependency management.


## Metal Model

This section will give you to mental primitives about how to think about how to interact with the tool. These two model might help you to create better workflow or write more specific prompts.

### The Big Brother

Working with vibe-coding tool can be thought as working with a very junior resource. The level of maturity required to execute the task is range between low-to-medium. You have to give them very specific instructions as you have to tell it what needs to be done, when and how. There is more hand-holding(code review). But mind you these junior engineers could lie or cheat to get things done, more task oriented.

One might argue newer models have more reasoning ability and the ability to poke in the environment, wouldn't that mean they are more  autonomous? Yes, agreed! but these models hallucinate too, they will do absolutely anything to complete the task. Filling in things even where they don't know what they are doing. That's why building a validation methods in the workflow is extremely important.

### Probabilistic vs Deterministic

Traditional programming follow what was call it as a deterministic way of executing task, large complex task is broken into small unit of work and each work takes a input and gives an output. The output is going to be the same if the input is same, Every single time! This is what we can say its deterministic/predictable. Out programming language have evolved to capture this essence of work and has given as sophisticated primitive to do it.

LLM's on the other way work very differently, the work in the probabilistic model. They trying to predict the next word based on your query/input/prompt. But if you give the same input to other model or an updated version of the same model the output might be different. The output will be influenced by training data of the model. Nonetheless it is called a probabilistic model.

The mental model will be used later to understand how we can combine both of these achieve much better outcomes.

## The Good Programmer

Now that we have mode some basic very clear, lets talk about the how can this be used in doing more complex task.

---

# Notes

- [ ] Break down large body of complex task in to smaller, more approachable units of work. Now, these smaller task we can be either accomplish by script(which give more deterministic output) or LLM(which can be a probabilistic output).
- [ ] We have to create a workflow in terms of "unit of work". A particular unit is doing very specific well define work, in which you give specific input and output defined. This "unit of work" can be converted into SKILL which the agent can deploy to execute.
- [ ] LLM Workflow - Explore, plan, code, validate.
- [ ] The new way of programming has shifted from learning a syntax of language to writing instruction in English. This is just like instructing a dump junior engineer.
- [ ] I have lost the fun of create and debug the program. The way I look at the program has changed completely, I spending more time reading code and its documentation. The I organize the program has also changed. The moat now is in new and novel idea, creating program will be come cheap.
- [x] A good mental model about vibe-coding is that you are working with a Junior engineer and you give it a very specific instruction. A voice interface can improve the quality of experience.
- [ ] The programming slit replay of algorithmic model, you have series of steps to execute stitch together of input-output. But instead of programming language, now it just natural language.
-  [ ] Help the LLM's to navigate the code just how you will navigate the code, only few variable and function and code paths at a time. You can only hold so much amount of information, how can you use this limitation to get thing done. Now both, you and LLM standing with same set of limitation but how can you instruct it with right amount of information to execute what you want? Use this question to craft your context. Context Engineering, they say!
- [ ] Vibe Coding
	- [ ] Documentation - Documentation is now include everything like how to setup environment, directory structure, architecture, etc.
		- [ ] Documenting how you program work can help you to minimize text required the create a proper context. Since you are specifying the interface input-output, or how to use the program and get things done instead of LLM reading code to understand what the program does.
		- [ ] Docs can be done by writing text description on what the program does and when to use it.
		- [ ] Give sample examples on how to use the script, like sample command and what does the output mean much like '--help' command output.
		- [ ] The consumer of this documentation is not going to be other engineers but also LLM Agents who will be creating more code, write test or generating other supporting documents. It become more important to do it and do it right!
	- [ ] Unit test cases - this help you validate the program which is written and when the LLM starting making any changes you can revert it back or debug it with the test cases.
		- [ ] Trusting LLM blindly is not a good strategy to use it, test case are one example of feedback which it can use it to debug.
		- [ ] LLM can produce a perfectly good looking code which is completely again what you asked it to do. Test driven development is good way to create binding contract on what need to be done and test it as and when your project evolves.
	- [ ] Dependency management - This is can be a quality of life improvement, because its possible and i have happened to me once that, You have written a piece of code which works well in your dev environment, but when you take to other environment and tell the LLM to run the code and it encounters the error and to fix it goes in to rewrite-debug loops and comes up the new solution. Once possible reason for this is the package version are different your dev environment as a certain version which the different from the new environment, may be some API's have change or new warning about API deprecation has started to pop up and LLM thinks it a good idea to fix it right now. Best way to avoid this is to lock the deps in virtual env or docker and let it LLM's work in such locked environment. Time and again try to run your program in new environments.
	- [ ] Version control - Plan you work with high level objective, but to progress you have to break it down into small manageable chunks you have created with with back and forth with LLM's has to checked-in this is important because LLM might make check to portion of check which you didn't intend to or you don't agree to what LLM have produced you would want to revert the change back and give it a fresh prompt which is much better than your previous attempt.
		- [ ] You can revisit the changes for review that, just how it was meant to be used.
		- [ ] It would be a good idea to let the LLM generate the summary of the commits and embed a small signature to show that the commit was made by LLM.
	- [ ] Much of what was discussed so far is already part of good engineering practices. But these practices can become a force multiplier; they help you to create better context engineering. The result of these good practices are not just consumed by other engineer's for ramp-up but also by LLM which are helping you to write more code. If you do this well you will treated with better code generation and less mistake.
	- [ ] Generating supportive scripts : like installing dependencies, runtime environment and test automations. These small help-scripts minimize your token limit consumption have deterministic out-come of you request.
	- [ ] Code Review - you will be reviewing code and docs all the time, because by virtue of using LLM's that what you asked for when you have someone else writing the code, the one who you can't fully trust, that what you have to do, you have to review their work. You have to validate this output but that ok! because you have saved yourself lots and lots of keystroke you may as well use that time somewhere more useful.
		- [ ] This is a good opportunity to completely rethink our code editor, as part of my work I was spending more time reviewing the diff code that it made me think, i should be rather look have a code editor which has good git integration.
	- [ ] Deterministic vs Probabilistic outcome - 
		- [ ] Model output can change based on the model you are working with, this kind of outcome might not be a good idea if decision are executed without a review. Such model outcomes may go through and additional reviews. We can say that such scenarios are Probabilistic is nature they depend on the model and how it thinks.
		- [ ] While on the other end programs are deterministic in nature the create same output for same set of input every single time no matter which environment you execute them in.
		- [ ] The best way to exploit this would be to use hybrid approach. Decompose workflow in different "work units" see if you can convert some of these task can be handled by a program and if not can convert the work in very strict instruction workflow which has additional validation.
		- [ ] What is the work you can out-source to LLM and what can be programmed. This will improve as you work more with LLM's and get good intuition of what can be out-sourced.
- [ ] Where do LLM fits in the whole SDLC?
	- [ ] Any software is basically layer of different components which together makes it a success. The more inner layer which is the core of the a product is very important and which is where you want to spend major or the time. Improving or refining it. 
	- [ ] LLM can help you drastically reduce the time you spend building on outer layer and instead spend more of you time in research and ideation of product and improving the core of your product. Because that's where the MOAT is!
	- [ ] Writing specifications, defining success criteria.
	- [ ] How can you create agents of create other task. digital interns-agents.