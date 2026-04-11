---
up: "[[Vibe Project MoC]]"
tags:
  - type/vibe-specs
created-date: 2026-04-08
related:
summary: Detail notes on Qgenie usage in Qualcomm
---

## Notes

- **Code Generation** - using the create project which would otherwise be required a developer. For example, to scale our fuzzing we are developing agentic harnessing
- **Learning assistance** - New way of learning, a lot of knowledge about system is locked in source and you won't find docs because code is the most updated and uncontested source of truth. I often open a coding agent in the source directory to just learn about the target project by asking question agent can navigate code and extract understanding of the project. Since a lot of our work involves review target, this is a very helpful tool.
- **Scaling** - There are some projects like code review, entry-point analysis for kernel driver. Call graph generation to understand attack surface.
- AI assisted code review - found some but mostly accidental and not too reproducible. 
- **Projects** - some of the project I have create a as follows
	- https://github.qualcomm.com/mshelia/llm_syzkaller - this project has some automation scripts to automate some of the syzkaller task, like grammar comparation.
		- adb mcp bridge - we have a device farm a we want to expose these devices to the agents.
		- device program - simple script to give agents the ability to poke kernel drivers.
		- CR Details - fetch CR details from the database.
			- the goals is to fetch bugs and update the harness to find the ourselves.
	- https://github.qualcomm.com/mshelia/entry_point_scanner - this is a upgraded version of entry-point analysis. this analysis figures-out entry-point of the kernel driver.
		- also create call-graph based on the entry point.
		- Feed the call-graph to the syzkaller. This creates directed fuzzing. This helps us to find the bug faster.
	- https://github.qualcomm.com/mshelia/attack_surface - this is the web application which scans the attack surface on different branches. 
		- Each project branch can have different code and we want to know the what is the attack surface for each branch.
		- Frontend- backend, job queue 
		- periodically fetch all the branches and scan the one requested by the users.
	- https://github.qualcomm.com/mshelia/device_monitor - this program focus on recovering the device from the bad state.
	- [zephyr firmware fuzzing](https://github.qualcomm.com/mshelia/mqtt_zephyr/issues/1) - fuzz the mqtt stack.
- Pattern - plan, create-review, cycle.

## Prompts

## Innovate
- What's the single smartest and most radically innovative and accretive and useful and compelling addition you could make to the plan at this point?

### ToDo 

Please do ALL of that now. Keep a super detailed, granular, and complete TODO list of all items so you don't lose track of anything and remember to complete all the tasks and sub-tasks you identified or which you think of during the course of your work on these items! use 'beads' to record the action items.

All the end of the epic attach a two task, one to verify the changes manually and other to write an automated test script for all possible scenario.

### Fix it

Check over each bead super carefully - are you sure it makes sense? Is it optimal? Could we change anything to make the system work better for users? If so, revise the beads. Its a lot easier and faster to operate in "plan space" before we start implementing these things! DO NOT OVERSIMPLIFY THINGS! DO NOT LOSE ANY FEATURES OR FUNCTIONALITY! Also make sure that as part of the beads we include comprehensive unit test and E2E test scripts with great, detailed logging so we can be sure that everything is working perfectly after implementation.

### Review

Can you review the changes you just make, make sure they are align with out goals, there is not bugs, error or incomplete work.

I need you to search every last INCH of this ENTIRE repo, looking intelligently for ANY signs or indicators that functions, methods, classes, etc. are "stubs" or "mocks" or "placeholders" or "TODO" or otherwise rather than 100% real, working, fully-functioning code.