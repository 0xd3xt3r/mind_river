---
up: "[[Tooling MoC]]"
tags:
  - "#type/tooling"
created-date: 2024-12-26
related: 
summary: Tools to improve code review 
---

## Requirements

1. Picture to display complex relation between entities
2. Code path tracing with notes
3. Documenting code without editing the code (code annotation)

## Code Annotation

- When reviewing the code you want to take notes without modifying the code
	- SciTool understand has this feature
	- vscode there are two plugin which we can use 
		- **Code Annotation** - plugin which allows you to take note without modifying the code
			- This the export the section in md format which can be useful for filing bug
		- **Better Comments** - this allows you to make comments with color code, you can also define you custom tags. But, you have to modify code file which is the downside. 
		- but they don't fully comply with the requirement, may be we should create our own plugin which merges above two features.
	- WeAudit - by Trails of Bits

## WeAudit

- [Project link](https://github.com/trailofbits/vscode-weaudit)
- Excellent tool to make note about code and issue
- To categorize code section Make the title as TAG. Example tags are 
	- Entry Point - to make the entry points of the project
	- Reachable - Reachable section of the code
	- Not Reachable - Unreachable of suspected code section

## PlantUML

- Generate diagrams from textual description
- link to the tool https://plantuml.com/


## Pandoc for slideshow

- theme - beamer

```markdown
---
title: "Windows Kernel Programming for Offensive Purpose"
author: "Munawwar Hussain Shelia (mshelia)"
institute: "QPSI | Product Security"
date: Nov 22, 2024
fontsize: 8pt
theme: "metropolis"
toc: true
toc-title: Agenda
---

## Title
```