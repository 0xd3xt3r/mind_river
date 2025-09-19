---
created-date: 2024-12-18T21:05
up: "[[Meeting MoC]]"
participants:
  - "[[Murali]]"
  - "[[Nagaraju]]"
  - "[[Bernard]]"
related: 
tags:
  - "#type/meeting"
  - "#qpsi/meeting"
---

> **summary**:: Generate call-graph for Android kernel driver.

## Notes

- [ ] [[Murali]] finding entry point function
	- all the function which doesn't have ancestor function is entry point suspect.
	- find all the descents of the entry point function that will be the overall denominator for the function.
	- We need to have 80 % coverage overall for now and 80% individual overall
- [ ] Generate the entry point function and maintain it([[Bernard]])
	- generate bitcode function.
- [ ] CRA fuzzing for all the product/interfaces. [[Murali]]
	- European Standards - Cyber Resilience Act (CRA)
- [ ] [[Murali]] For CRA requirement we need to fuzz all product. 
	- For timeline New fuzzing 6 months and old fuzzing product 3 months.
	- any identified interface and product and fuzz them.
- [ ] fuzzing only for premium tier not on value tier will be later part of value CRA

## Action Item

- No Action item on my side.