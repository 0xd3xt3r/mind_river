---
up: "[[Writing MoC]]"
tags:
  - "#type/writing/article"
status: in-progress
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
	2. You can write a query for the tool which is matching a variance of the existing know bug.
2. The disadvantage of static analysis are as follows
	1. you need to spend lot of computation on the scans. 
	2. The scan are looking for pattern, so essentially you will only get what you are looking for, you cannot not expect example of variance. Then there is the problem of false positive.
	3. You have to add support for the build systems.
	4. State explosion.
	5. The number of reports are so high that no-one cares to sort them out. Also you have to filter out the false-positive.
	6. In terms of cost you need to consider three components:
		1. software licensing cost, 
		2. People who are running the scans infrastructure, the dev-ops part of the thing.
		3. People who are add support for the builds for a software components for the tool
		4. The infrastructure cost of running the tool on the code. The computation cost.
		5. The human analyst creating the scan queries or the patterns.

### Code Review

1. Code review been the most manual of previous two technique but it is the most involved and it has yield one of the most impactful bugs.
2. This is the fundamental set to any security assessment activity and should be used to extract intelligence and feed it to the other two techniques.

### Variant analysis

1. [link](https://github.blog/security/vulnerability-research/multi-repository-variant-analysis-a-powerful-new-way-to-perform-security-research-across-github/)
2. Variant analysis is an important technique for identifying new types of security vulnerabilities. The static analysis engines that powers code scanning, is a great tool for variant analysis because it allows you to model vulnerabilities as highly precise custom queries.

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

Security bugs intel flows in our system is by means of external security reports. This serves as a very important feedback mechanism to our system, based on this feedback start security activity on particular module, with the goal on minimize the bugs in that component.

## Misc

1. Patch verification - how do we know the fix provided by the developer address the issue
	1. Validating Patch Effectiveness: When a patch is applied, it's meant to fix known issues. But how do you know it hasn’t introduced new ones?
2. Patch propagation - once we know the bug, we have to fix the on all the product on which the software is running.
3. Gap-analysis - once the vulnerability is know, there time it take to be shipped to the customer.

## The Ideal world

I often contemplate on the question, if we have a world where there were not software vulnerability, what would that be? What sort of tool and technique would that parallel universe would have which has complete taken over those vulnerability?

The answer would have been there must be a program that could autonomy run for the target and figure out the issues and fix it for itself. But still what would be a defining characteristics of those program? It would have following features
1. Spidey-sense like intelligence to find all the incoming point of attacks. (Threat Model)
2. A very powerful taint analysis engine - this thing can help you to trace the data incoming from attack point to potential weak spots on the systems.
3. The ability to create operational environment for the target which replicates the production environment. This simulated environment help you to validate the vulnerability and increase the confidence of the exploitability. Reproduceable bugs can be validated.
4. The ability to create semantic understanding of the input to the attack surface and create a specification of the input which it will which can be given to the mutation engine to bombard attack surface. The more precise the specification is the better the mutation. 
	1. The semantics of the system will also create specification which pin-points corner-cases of the system.
	2. Understand the state-machine of the system.
5. Observability of the systems - The ability to observe the system's behavior while it is processing the data or running. This ability will not only be used a feedback for specification but also the observer invalid behaviour and pin-point to that behaviour. This input can also help to improve the taint-analysis system.
6. Expected and un-expected behavior of the system - the specification of the system behaviour can be help use to determine the unexpected behaviour or invalid behaviour. In the context of security, buffer overflow is unexpected behaviour.
	1. Unit-test is one form of specification of the system.

We try to achieve some of this characteristic in automated fashion in different techniques like.
1. for static analysis - we have ability to reason base on semantics of the syntax what is the expected and un-expected behaviour.
2. Using debugger, sanitizers or instrumentation engine we are trying to create better observability of the system.
3. By fuzzing we trying to take input semantics and crafting semi-malformed input to poke the corner cases of the system and trigger the unexpected behaviour. But the change this technique faces is the execution environment.
4. The code review is the step which is trying to create a semantic understand of the system in the analyst mind, 

## Security Trainings

- Its very important to have a secure coding training for the developer who are writing the code for the organization. Such training will not only help them to improve their understand of the different vulnerability class but also how to correctly fix them. If they don't have the foundational knowledge they will the knowledge gap when they are fixing the security bug or when the security issue is reported to them or even while there is some issue caught while debugging or development.
- The training will help them to understand various aspect of the security. There are different level of security requirement depending upon the software modules. A generic "C language" issue training can help across the board, wherever you are writing the C code. But there could be platform specific training for example Linux kernel driver security which help the developers to understand privilege boundaries and other exploit primitives.
- The best way to carry out this exercise is to have video trainings and exam following this training. Video training are scalable and the developer join the team at anytime.

## Product Security Objective

Product Security organisation(DefSec) work very differently compared to offensive security groups(OffSec):
1. DefSec have to find all the bugs in the target to claim the victory while OffSec groups can have a winning business if they find a exploitable bug.
	1. But it not that easy these day to compromise a device with just one bug you need chain of bugs to bypass the mitigation which modern devices deploy.
2. DefSec can't claim the victor just yet because the patches have to propagated to the software module, tested for the new changes and shipped to the customer device/product. The more stretched the timeline is for this activity the better it is for the OffSec group. In fact it better of OffSec guys the more stretched timeline they have. For OffSec forks if the find the exploitable bug and it not patch the better off they are!
3. The product security activity described before is just a snapshot in time, as new code gets added/remove security security assessment needs to be done again. DefSec forks might have to optimize their efforts on how to spend their bandwidth on what? This is also true for OffSec guys this might be a new opportunity to find bugs in a secured target or it might make old unreachable bugs more relevant and might turn they more fruitful.
4. DefSec guys have to report their findings to public and their vendor, the OffSec guys can perfectly get away with it. Jokes apart, the information revealed from DefSec can be used by to find new bugs by doing variant analysis, etc. The patching activity could in fact introduce new bug. While for the OffSec forks don't have those limitation, they can share the information with close group to protect their attacks and the maximize their economic value of the efforts.

## Measuring the Security outcomes

1. Representing fuzzing coverage as function of the attack surface for an image. This assumes that we have put in thought on attack surface enumeration. I can understand some "unknown" attack surfaces. Also, there will be some known surfaces with no harness like fbe() and tghr()
	1. ![[attack-surface-graph-example.png]]
2. As I see it, we’d need the tool based on either LLVM or KW that’d provide a call graph for a given entrypoint.
	1. Maybe for each major image (like WLAN/MPSS) we should document the set of heuristics to look for parsers (like here https://confluence.qualcomm.com/confluence/pages/viewpage.action?pageId=573375230 ) and then this can either be automated or someone has to do it once a few months.
3. Alex
	1. How do I read the chart?  Functions on the x-axis and coverage on the y-axis?
	2. What makes it a map of the attack surface?  How is a single function with a giant switch on the message type differ from 100 different functions (one per message type)?
	3. How do we plan to use this data to influence our decisions?