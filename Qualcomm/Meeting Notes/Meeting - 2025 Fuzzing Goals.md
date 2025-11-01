---
up: "[[Meeting MoC]]"
related:
participants: "[[Nagaraju]]"
created-date: 2025-02-12T11:13
tags:
  - "#type/meeting"
  - "#qpsi/meeting"
summary: Fuzzing goals and though process for 2025.
---

## Meeting Minutes

- **Put in place infrastructure and processes for a fuzzing farm for 3 generations of premium-tier and value-tier Android, WoS and Auto products**
    - Plan and acquire hardware, rack-space and inter-connects to set-up and monitor continuous fuzzing on previous generation devices
    - Maintain harnesses by SP and fuzzing-target in version control
    - Maintain fuzzing corpus by SP and fuzzing target
    - Monitor for crashes, raise CRs and work with development teams to provide first patches
    - Defines KPIs to measure progress monthly
- **Improve security awareness of tech-teams**
    - Support the development of the "Secure Code: Windows Kernel" training
- **Ensure Modem MOBs and Linux kernel drivers have call-graphs to understand fuzzing progress**
    - Troubleshoot and produce call graphs for all fuzzing targets as needed for reporting
- Identify the fuzzing targets with
	1. Critical and High security Sensitivity (see definition for CRA) that also 
	2. had a significant change in anticipated QC contribution in the current calendar year, and 
	3. in the premium tier-SP, and report on the progress of fuzzing by counting the number of targets that are at each of the following maturity stages. out of the total number of targets.
		1. FR created & approved
		2. Attack surface defined
		3. Harness being updated to cover the interfaces exposed by the fuzzing target and version controlled by SP and target
		4. CRs being reported and tracked on the time from creation to first fix date
		5. Function coverage being tracked
		6. Regression suite automated from the latest fuzzing corpus
		7. Minimum time to crash is tracked and is increasing with time with commensurate increase in corpus-size

The metric is essentially the distribution of targets, Critical and High ones separately, across the maturity stages.

## Action Item

- 