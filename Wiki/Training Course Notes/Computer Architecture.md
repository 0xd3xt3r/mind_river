---
up: "[[Learning MoC]]"
tags:
  - "#computer-architecture"
  - "#learning/course"
  - "#type/knowledge"
aliases:
  - Onur Mutlu
  - Architecture
author: Onur Mutlu
status: todo
created-date: 2024-12-15
related: 
summary: Computer Architecture leatures by Onur Mutlu
---

## Learning Resource
1. [Onur Mutlu Lectures, Channel](https://www.youtube.com/@OnurMutluLectures/playlists)
1. [Onur Mutlu Lectures, ETH Zurich, Fall 2018](https://www.youtube.com/watch?v=g3yH68hAaSk&list=PL5Q2soXY2Zi9JXe3ywQMhylk_d5dI-TM7&ab_channel=OnurMutluLectures)
	1. 38 Lecture videos
	2. Digital Circuits lectures
2. [Onur Mutlu Lectures, ETH Zurich, Fall 2022](https://www.youtube.com/watch?v=BIpPTqHK-Lc&list=PL5Q2soXY2Zi-cAls3cyauNzM7-74Eq31O&pp=iAQB)
3. [Digital Design and Computer Architecture Onur Mutlu Lectures, ETH Zurich, Fall 2023](https://www.youtube.com/watch?v=VcKjvwD930o&list=PL5Q2soXY2Zi-EImKxYYY1SZuGiOAOBKaf&pp=iAQB)
4. [Computer Architecture, Onur Mutlu Lectures, ETH Zurich, Fall 2024](https://www.youtube.com/watch?v=ziMRjDlLEwo&list=PL5Q2soXY2Zi-LfDdGgWyLcTSqzm6a26wD)
5. [Digital Design and Computer Architecture - Spring 2023](https://www.youtube.com/playlist?list=PL5Q2soXY2Zi-EImKxYYY1SZuGiOAOBKaf)

## Notes 

1. Three components Computation, Storage & Communication
2. [Computer Architecture - Lecture 3: Memory Systems: Trends, Challenges, Opportunities Fall - 2021](https://www.youtube.com/watch?v=W6tDBt0prAM&list=PL5Q2soXY2Zi-Mnk1PxjEIG32HAGILkTOF&index=3)
3. **Parallelism & Heterogeneity**
4. **Timing & Verification** - Clock Cycles/ Chip Clock
	1. Lect 6 - in Digital & Comp Arch 2023
5. **Interconnects** - 
	1. [Computer Architecture - Lecture 21: Interconnects - Fall 2021](https://www.youtube.com/watch?v=P_57FZSnrO4&list=PL5Q2soXY2Zi-Mnk1PxjEIG32HAGILkTOF&index=24)
	2. Connect and comm between components, Those components could be:
		1. Processor to Processor
		2. Processor to memories(Banks)
		3. Processor to Caches
		4. Cache to Cache
		5. I/O Devices
	3. Topology - How does the network look like
		1. Bus - simple
			1. not scalable, limited bandwidth.
		2. Crossbar - 
	4. Routing - How does message gets from source to destination.
	5. Buffering & Flow Control - How to you handle throttling
	6. Terms
		1. Network Interface - 
		2. Link - bundles of wires that carry a signal
		3. Switch 
		4. Channel
7. **On-Chip Network/Network on Chip (NoC)**
8. **Memory Interference**
	1. one application usage of memory resource can affect the performance of other application because of shared cache memory
	2. Data centers have service level agreement for this to block cores.
	3. Uncontrollable, unpredictable systems.
	4. Three resource to make QoS possible, Memory Controllers, interconnects & Caches
	5. *Source throttling* - control the injection of request from the source into the shared resource. If the source is hogging too much resource the source is said to hold-off.