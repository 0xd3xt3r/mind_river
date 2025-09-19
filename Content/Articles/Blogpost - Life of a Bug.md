---
up: "[[Writing MoC]]"
tags:
  - "#type/writing/article"
status: todo
created-date: 2025-09-13
---

Software bugs are living creatures, they are born they have a life in the software they reside and with enough efforts they can be killed but they cannot be completely eradicated. But what I was curious was to understand the life cycle of the bug and what sort of mitigation can be put in place to address they at different stage.

Lets start when the bug is born, it but out of ignorance of software developer and the life of bug is extended in the code base because of their arrogance. The longer it is alive in the code-base the more it is difficult to remove because the code shipped it different product and customer and to propagate the bug it becomes more tedious to propagate the fixes and ship those fixes to the customers device.

There are various tools and technique which one can use to detect the bugs finding the bug

Well there are different techniques which are used to find the bugs like fuzzing, static analysis, code review. 

### Fuzzing

1. Fuzzing required you to understand the code base you have to put in lot of effort of get the target running or get the fuzzing data to the code you want to test. 
	1. But the advantage of this technique is that it is scale-able you can start your project with 8 core 16 GB ram and still expect some results. 
		1. Fuzzing has yield very good results and have been adopted really well in the industry.
	2. But the disadvantage of this technique 
		1. is that you have to actively maintain the harness as the target code keeps updating your harness.

### Static Analysis

1. Advantage 
	1. this technique scales in terms of harnessing, once you have configured and written the rules for the scan the tool will keep monitoring those patterns. 
2. The disadvantage of static analysis is 
	1. you need to spend lot of computation on the scans. 
	2. The scan are looking for pattern, so essentially you will only get what you are looking for, you cannot not expect example of variance. Then there is the problem of false positive. Another advantage 

### Code Review

Code review been the most manual of previous two technique but it is the most involved and it has yield one of the most impactful bugs.

### Mitigations

Another technique is enabling mitigation like ASLR, CFI, DEP, etc. This techniques help you have security even in case the bug is triggered.

The best way to address the problem is detect the bug during the development phase, right when the code is written or modified. The ideal scenarios would be that any of the mentioned technique raises an alert in the developers code-editor and gives suggestions to the developer on what needs to be done to fix the issue. That's an Ideal scenario.