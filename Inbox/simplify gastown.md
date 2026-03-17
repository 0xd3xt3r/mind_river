---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/knowledge"
status: todo
created-date: 2026-03-15
related: 
summary: Extract out some of the good practises of the gastown project and create our workflow such that it token-budget friendly
---

## Important formulas

1. mol-plan-review-formula.toml
2. mol-idea-to-plan 
3. mol-prd-review 
4. design

## Prompt

I want to remove gastown dependency for certain formulas. I want you to convert the formula mol-idea-to-plan, mol-prd-review, mol-plan-review, and design to execute just with beads mol and cook command. For this you might have to covert type convoy to expand type and make sure the task a are parallel. There has to be a consolidation phase for all the parallel task which is mentioned in the mol-idea-to-plan formula. Cover each stage of the 'Pipeline' and in some phase 'polecate' is mentioned which is has to be borken down into the formula file. like prd-align, plan-review, create-beads. Preseve the sequeuce of the formula. If there are conditional block then create formula assuming the condition is true.

Create all the output in formula folder.

Refer to @workflow_plan.md and @session-formula.md for previous effort on this problem.

plan-review and prd-align should have single consolidation/synthesis step.

Write a tutorial about the how to use these formulas along with the 'bd' commands.

What's the single smartest and most radically innovative and accretive and useful and compelling addition you could make to the plan at this point?