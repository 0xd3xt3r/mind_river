---
up: "[[Meeting MoC]]"
related: 
participants:
  - "[[Ashish]]"
  - "[[Nagaraju]]"
created-date: 2025-07-02T16:00
tags:
  - "#type/meeting"
  - "#qpsi/meeting"
---

> **summary**:: Roadmap for QOT Fuzz

## Meeting Minutes

1. Requirement
	1. Running multiple devices
	2. Infrastructure
2. Issues
	1. Not scalable for user(fuzz client size). We need One lab machine per device.
		1. Rebooting, updating and machine is not reliable.
	2. Coverage guided fuzzing is not implemented yet.
		1. How to get coverage?
		2. We don't know how to evaluate our efforts.
		3. Bullseye can instrument only one driver.
		4. Display the coverage information.

## Action Item

1. Documentation for the project