---
up: "[[Writing MoC]]"
tags:
  - "#type/writing/article"
status: todo
created-date: 2025-09-13
---

Software bugs are living creatures, they are born they have a life in the software they reside and with enough efforts they can be killed but they cannot be completely eradicated. But what I was curious was to understand the life cycle of the bug and what sort of mitigation can be put in place to address they at different stage.

Lets start when the bug is born, it but out of ignorance of software developer and the life of bug is extended in the code base because of their arrogance. The longer it is alive in the code-base the more it is difficult to remove because the code shipped it different product and customer and to propagate the bug it becomes more tedious to propagate the fixes and ship those fixes to the customers device.

## Security Tooling

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

### Mitigation Features

Another technique is enabling mitigation like ASLR, CFI, DEP, etc. This techniques help you have security even in case the bug is triggered.

The best way to address the problem is detect the bug during the development phase, right when the code is written or modified. The ideal scenarios would be that any of the mentioned technique raises an alert in the developers code-editor and gives suggestions to the developer on what needs to be done to fix the issue. That's an Ideal scenario.

## How do bugs get propagated?

It is very important to understand how bug is propagated, once the new code is tested and added to the main branch these changes are then picked up for software product in the form of a patch. These patches are then shared with all the vendors. The vendor then integrate this software components in their products and with sufficient testing they ship it to the customer. Now imagine one of the patch is security bug and it goes undetected till it reaches the customer. It will take same amount of time and effort it took to ship the new feature from developer to customer.

## What to do with the bugs?

The first thing to do once the bug is detected with to find all the branches on which this bug is present. This is done by tools which scan for the vulnerable snippet of code. If the match is found the branch if flagged. The corresponding development team is notified or if possible patch can be automatically applied.

Automated application of the patch is possible if the test cases are available. The test suite can help you validated if the patch is not faulty. Another approach could be to do a small grey box fuzzing to testing the patch.

This fuzzing could be done to see if the code section on which the patch is applied is fuzzy tested for couple of times, i.e. if the path is saturated for enough time. To make sure the desired path is executed you have to custom feedback mechanism to favour test cases trigger patches code section. This is not a deterministic solution but i does give you some level of confidence.

There are many reason for not fixing a bug, some of which are:
1. The bug has been under-rated and hence it not considered to be that important to be fixed and propagated everywhere.
2. The bug propagation itself is very costly process where the developer has to manually fix apply the patch and do the testing of this code. The burdensome part of this process is because we don't have good test-suite to automatically test new changes and hence manual intervention is required.
3. If the issue is reported externally it comes with a deadline from the security researcher. This kind of deadline are not welcomed by software development team because they already have projected the plans for next quarter/year.


## Security as a Systems

Lets considers QPSI as a System who's purpose is to create secure products. The stock of the system is the security vulnerability( security bugs) in software products. Once we see the bugs rising and our goal is to minimise this stock my various activities like fuzzing, static analysis, code review, etc. The in-flow of the system is fault software patches which increases the stock of bug in our product. The bugs get propagated to various products along with other patches.

Security bugs intel flows in our system is by means of external security reports. This serves as a very important feedback mechanism to our system, based on this feedback start security activity on particular module, with the goal on minimise the bugs in that component. 