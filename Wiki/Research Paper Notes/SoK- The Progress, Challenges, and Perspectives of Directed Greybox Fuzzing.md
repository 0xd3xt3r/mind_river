---
up: "[[Research Paper MoC]]"
tags:
  - "#reading/research-paper"
status: todo
created-date: 2024-12-16
---

> **summary**:: Greybox fuzzing Survey paper

## Core Idea


## Notes


- [Paper Link](https://arxiv.org/pdf/2005.11907.pdf)

- GreyBox Fuzzing Most greybox fuzzing tools are coverage-guided
- directed greybox fuzzing (DGF) spends most of its time allocation on reaching specific targets (e.g., the bug-prone zone) without wasting resources stressing unrelated parts
- DGF is particularly suitable for scenarios such as patch testing, bug reproduction, and special bug detection.
- Greybox fuzzing generates inputs by mutating seeds. By specifying a set of target sites in the PUT(program under test) and leveraging lightweight compile-time instrumentation, a directed greybox fuzzer can use the distance between the input and the target as the fitness function to assist seed selection
- By giving more mutation chances to seeds that are closer to the target, it can steer the greybox fuzzing to reach the target locations gradually. Unlike traditional fuzzing techniques, DGF casts reachability as an optimization problem whose aim is to minimize the distance between generated seeds and their targets [18]. Compared with directed symbolic execution, DGF has much better scalability and improves efficiency by several orders of magnitude. For example, B¨ohme et al. can reproduce the Heartbleed vulnerability within 20 minutes, while the directed symbolic execution tool KATCH [17] needs more than 24 hours [18]

- The data extracted from each paper will be:
	- The publication source (i.e. the conference, journal, or preprint) and year.
	- The fitness goal. To reach what kind of target sites (e.g., vulnerable function) or to expose what target bugs?
	- The fitness metric used in the evolutionary process of fuzzing. For example, the distance to the targets.
	- How the targets are identified or labeled? For example, predicted by deep learning models.
	- The implementation information. What tool is the fuzzer implemented based on? Is the fuzzer open-sourced?
	- Whether the tool supports binary code analysis?
	- Whether the tool supports kernel analysis?
	- Whether the tool supports multi-targets searching?
	- Whether the tool supports multi-objective optimization?

### Fitness Goal

This section tried to summarize what are the way in which the seed corpus match is calculated:
Fitness Metrics The crux of DGF is using a fitness metric to measure how the current fuzzing status matches the fitness goal, so as to guide the evolutionary process. We summarize the following fitness metrics used in DGF. 
1. Distance: Based on the investigation, 31% (13/42) of the directed greybox fuzzers follow the scheme of AFLGo by using the distance between the input and the target as the fitness metric. AFLGo [18] instruments the source code at compile-time and calculates the distances to the target basic blocks by the number of edges in the call and control-flow graphs of the PUT. Then at run-time, it aggregates the distance values of each basic block exercised to compute an average value to evaluate the seed. It prioritizes seeds based on distance and gives preference to seeds that are closer to the target. Some follow-ups also update this scheme.
2. TOFU’s distance metric is defined as the number of correct branching decisions needed to reach the target [70].
3. RDFuzz [69] combines distance with execution frequency of basic blocks to prioritize seeds.
4. UAFuzz [25] uses a distance metric of call chains leading to target functions that are more likely to include both allocation and free functions to detect complex behavioral use-after-free vulnerabilities.
5. One drawback of the distance-based method is that it only focuses on the shortest distance, and thus longer options might be ignored when there is more than one path reaching the same target, leading to a discrepancy

## Connected Concepts

1. Ant Colony Optimization [link](https://smartmobilityalgorithms.github.io/book/content/SwarmIntelligenceAlgorithms/AntColonyOptimization.html)

### Ref
