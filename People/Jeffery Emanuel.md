---
up: "[[People MoC]]"
full-name: Jeffery Emanuel
created-date: 2026-03-15
tags:
  - "#type/person"
summary: AI pioneer
related:
  - "[[Vibe Project MoC]]"
---

## Notes

- Authors prompt library https://jeffreysprompts.com/
- https://jeffreyemanuel.com/
- https://agent-flywheel.com/

## Accounts

- [Twitter](https://x.com/doodlestein/)
- [LinkedIn]()
- [Github](https://github.com/Dicklesworthstone)


## Prompts

### Innovative ideas

When you think you're finished with your development plan for your agent, try this prompt with a few different frontier models. You might be amazed what they come up with:

"What's the single smartest and most radically innovative and accretive and useful and compelling addition you could make to the plan at this point?"

If you've already started development and have a fleshed out project already, replace the word "plan" with "project."

### Getting Thinks Done

#### P1

Please do ALL of that now. Keep a super detailed, granular, and complete TODO list of all items so you don't lose track of anything and remember to complete all the tasks and sub-tasks you identified or which you think of during the course of your work on these items! use 'beads' to record the action items.

All the end of the epic attach a two task, one to verify manually and other to write test script for all possible scenario.

#### P2

OK so please take ALL of that and elaborate on it and use it to create a comprehensive and granular set of beads for all this with tasks, subtasks, and dependency structure overlaid, with detailed comments so that the whole thing is totally self-contained and self-documenting (including relevant background, reasoning/justification, considerations, etc.-- anything we'd want our "future self" to know about the goals and intentions and thought process and how it serves the over-arching goals of the project.). The beads should be so detailed that we never need to consult back to the original markdown plan document. Remember to ONLY use the `bd` tool to create and modify the beads and add the dependencies.

#### P3 - Super granular

OK good, now I need you to come up with an absolutely comprehensive, detailed, and granular plan for addressing each and every single one of those placeholders/mocks/stubs that you identified in the most optimal and clever and sophisticated way possible. 

THEN: please take ALL of that and elaborate on it and use it to create a comprehensive and granular set of beads for all this with tasks, subtasks, and dependency structure overlaid, with detailed comments so that the whole thing is totally self-contained and self-documenting (including relevant background, reasoning/justification, considerations, etc.-- anything we'd want our "future self" to know about the goals and intentions and thought process and how it serves the over-arching goals of the project.) The beads should be so detailed that we never need to consult back to the original markdown plan document. Remember to ONLY use the `bd` tool to create and modify the beads and add the dependencies.

### Fix your beads/task

Check over each bead super carefully - are  you sure it makes sense? Is it optimal? Could we change anything to make the system work better for users? If so, revise the beads. Its a lot easier and faster to operate in "plan space" before we start implementing these things! DO NOT OVERSIMPLIFY THINGS! DO NOT LOSE ANY FEATURES OR FUNCTIONALITY! Also make sure that as part of the beads we include comprehensive unit test and E2E test scripts with great, detailed logging so we can be sure that everything is working perfectly after implementation.

## Testing

We really need to have totally complete, totally comprehensive, granular, perfect end to end testing coverage without ANY mocks or fake data, fake api calls, etc., that prove that our entire pipeline from start to finish works perfectly in a provable, ultra rigorous way. That means the raw data coming in from the various API services for EVERYTHING (not just one or two fields) for a bunch of test cases. Basically, the WHOLE thing, from "soup to nuts".


## Mock Code elimination

First read ALL of the AGENTS.md file and README.md file super carefully and understand ALL of both! Then use your code investigation agent mode to fully understand the code and technical architecture and purpose of the project.

Then, I need you to search every last INCH of this ENTIRE repo, looking intelligently for ANY signs or indicators that functions, methods, classes, etc. are "stubs" or "mocks" or "placeholders" or "TODO" or otherwise rather than 100% real, working, fully-functioning code.

You can apply a variety of methods for checking for this, but it's imperative that you not miss ANY instances of this sort of thing. One clever way might be to use ast-grep to find and measure the length of any functions/methods/classes/etc. in terms of lines, characters, etc. to look for things that look suspicious because they appear to be too short to do anything substantive.

First compile the comprehensive listing of all such placeholders/mocks/stubs and a short explanation or justification for why you're convinced they qualify as incomplete/placeholders that must be completed. Once we have this table of suspects, we can then decide how to address and resolve them all in a totally comprehensive, optimal, clever way.