---
created-date: 2024-12-19T12:03
up: "[[Meeting MoC]]"
participants:
  - "[[Neel Patwa]]"
related: 
tags:
  - "#type/meeting"
  - "#qpsi/meeting"
---

> **summary**:: LLM for klockwork issue classification

## Meeting Minutes

- LLM for [[Inbox/Klockwork]] issue classification
- 40-50 % false positive and manual classification.
- Identify the issue and propose proper fixes
- Purpose automated fix.
- Also use CR data and target for automated fix.
- Architecture
	- ChatWise SDK which has Code llama model
- Not fine tuning and training llama model its just uploading the data.
- Using LLM to parse function trace in issue and use that for classification.

## Action Item