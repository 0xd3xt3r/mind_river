---
up: "[[Reading MoC]]"
tags:
  - "#vulnerability-research"
  - "#type/reading/book"
aliases:
  - security assessment
  - software security assessment
status: reading
author: Mark Dowd, John McDonald, Justin Schuh
title: The Art of Software Security Assessment - Identifying and Preventing Software Vulnerabilities
created-date: 2024-12-14
summary: This is one of the most detailed, sophisticated, and useful guides to software security auditing ever written
related:
  - "[[Learning MoC]]"
---

A piece of software might have complexity that rivals any physical hardware, but most people never see its wheels spin, hear the hum of its engine, or take apart the nuts and bolts to see what makes it tick.

## Chapter 1: Software Vulnerability Fundamentals

- You’re no doubt familiar with software **bugs**; they are errors, mistakes, or oversights in programs that result in unexpected and typically undesirable behavior.
- Security vulnerabilities are bugs that pack an extra hidden surprise.
- Almost all security vulnerabilities are software bugs, but only some software bugs turn out to be security vulnerabilities. A bug must have some security-relevant impact or properties to be considered a security issue; in other words, it has to allow attackers to do something they normally wouldn’t be able to do.

### Security Policies

- *Security of a system* is to think of a system’s security as being defined by a security policy
- Violation of a Software system's security occurs when the system’s security policy is violated.
- For a system composed of software, users, and resources, you have a *security policy*, which is simply a list of what’s allowed and what’s forbidden.
- The term “security policy” often means the user community's consensus on what system behavior is allowed and what system behavior is forbidden.
- Every software system can be considered to have a security policy. It might be a formal policy consisting of written documents, or it might be an informal loose collection of expectations that the software’s users have about what constitutes reasonable behavior for that system

### Security Expectations

- **Confidentiality**: Confidentiality requires that information be kept private. for eg Financial information is generally expected to be kept confidential
	- If a software system maintains information about people, expectations about the confidentiality of that data are often high. Because of privacy concerns, organizations and users expect a software system to carefully control who can view details related to people.
- **Integrity**: is the trustworthiness and correctness of data. It refers to expectations that people have about software’s capability to prevent data from being altered.
- **Availability**: is the capability to use information and resources.

### Classifying Vulnerabilities

-  vulnerability classes are just mental devices for conceptualizing software flaws. They are useful for understanding issues and communicating that understanding with others, but there isn’t a single, clean taxonomy for grouping vulnerabilities into accurate, nonoverlapping classes. It’s quite possible for a single vulnerability to fall into multiple classes, depending on the code auditor’s terminology, classification system, and perspective.
- Some software vulnerabilities are best tackled from a particular perspective. For example, certain flaws might best be approached by looking at a program in terms of the interaction of high-level software components; another type of flaw might best be approached by conceptualizing a program as a sequence of system calls.

#### Design Vulnerabilities

> With a Design Flaw, the software isn't secure because it does exactly what it was designed to do; it was simply designed to do the wrong thing!

- Requirements usually address what a software system has to accomplish.
- Specifications are the plans for how the program should be constructed to meet the requirements.
- When people speak of a design flaw, they don’t usually make a distinction between a problem with the software’s requirements and a problem with the software’s specifications.

#### Implementation Vulnerabilities

- In an implementation vulnerability, the code is generally doing what it should, but there’s a security problem in the way the operation is carried out. As you would expect from the name, these issues occur during the SDLC implementation phase, but they often carry over into the integration and testing phase. These problems can happen if the implementation deviates from the design to solve technical discrepancies. Mostly, however, exploitable situations are caused by technical artifacts and nuances of the platform and language environment in which the software is constructed. Implementation vulnerabilities are also referred to as low-level flaws or technical flaws.

#### Operational Vulnerabilities

Operational vulnerabilities are security problems that arise through the operational procedures and general use of a piece of software in a specific environment. One way to distinguish these vulnerabilities is that they aren’t present in the source code of the software under consideration; rather, they are rooted in how the software interacts with its environment. Specifically, they can include issues with configuration of the software in its environment, issues with configuration of supporting software and computers, and issues caused by automated and manual processes that surround the system.


#### Gray Areas

The distinction between design and implementation vulnerabilities is deceptively simple in terms of the SDLC, but it’s not always easy to make. Many implementation vulnerabilities could also be interpreted as situations in which the design didn’t anticipate or address the problem adequately.


### Common Threads

#### User Input: Control Flow and Data Flow

- The majority of software vulnerabilities result from unexpected behaviors triggered by a program’s response to malicious data. 
- So the first question to address is how exactly malicious data gets accepted by the system and causes such a serious impact.
- this malicious data might come into play through a far more circuitous route than direct user input. This data can come from several different sources and through several different interfaces.
- When reviewing a software system, one of the most useful attributes to consider is the flow of data throughout the system’s various components.
- This process of tracing data flow is central to reviews of both the design and implementation of software. User-malleable data presents a serious threat to the system, and tracing the end-to-end flow of data is the main way to evaluate this threat. Typically, you must identify where user-malleable data enters the system through an interface to the outside world, such as a command line or Web request. 
- Then you study the different ways in which user-malleable data can travel through the system, all the while looking for any potentially exploitable code that acts on the data. It’s likely the data will pass through multiple components of a software system and be validated and manipulated at several points throughout its life span.
- This process isn’t always straightforward. Often you find a piece of code that’s almost vulnerable but ends up being safe because the malicious input is caught or filtered earlier in the data flow. More often than you would expect, the exploit is prevented only through happenstance; for example, a developer introduces some code for a reason completely unrelated to security, but it has the side effect of protecting a vulnerable component later down the data flow. Also, tracing data flow in a real-world application can be exceedingly difficult. Complex systems often develop organically, resulting in highly fragmented data flows.
- One reason this data can cause such trouble is that software often places too much trust in its communication peers and makes assumptions about the data’s potential origins and contents.

#### Trust Relationships

- Different components in a software system place varying degrees of trust in each other
- Trust relationships are integral to the flow of data, as the level of trust between components often determines the amount of validation that happens to the data exchanged between them.
- This means they generally believe that the trusted component is unaffected to malicious interference, and they feel safe in making assumptions about that component’s data and behavior.

#### Assumptions and Misplaced Trust

- Developers can make incorrect assumptions about many aspects of a piece of software, including the validity and format of incoming data, the security of supporting programs, the potential hostility of its environment, the capabilities of its attackers and users, and even the behaviors and nuances of particular application programming interface (API) calls or language features.

#### Interfaces

- Many vulnerabilities are caused by developers not fully appreciating the security properties of these interfaces and consequently assuming that only trusted peers can use them.
- misplace trust in an interface for the following reasons:
	- They choose a method of exposing the interface that doesn’t provide enough protection from external attackers.
	- They choose a reliable method of exposing the interface, typically a service of the OS, but they use or configure it incorrectly.
	- They assume that an interface is too difficult for an attacker to access. Proprietary systems.

#### Environmental Attacks

- Some software flaws occur when an attacker manipulates the software’s underlying environment. These flaws can be thought of as vulnerabilities caused by assumptions made about the underlying environment in which the software is running. Each type of supporting technology a software system might rely on has many best practices and nuances, and if an application developer doesn’t fully understand the potential security issues of each technology, making a mistake that creates a security exposure can be all too easy.

#### Exceptional Conditions

- **exceptional condition** occurs when an attacker can cause an unexpected change in a program’s normal control flow via external measures.
- This behavior can entail an asynchronous interruption of the program, such as the delivery of a signal.

## Chapter 2: Design Review

1. **Trust Domains**
2. **Chain of Trust** - it means if components A trusts component B the component A must implicitly trust all the components trusted by component A.
3. Defense in Depth - is the concept of layering protection so that compromising one aspect of the system is mitigated by other component. For eg a network service 


## Chapter 3: Operational Review

Operational vulnerabilities are the result of issues in an application’s configuration or deployment environment. These vulnerabilities can be a direct result of configuration options an application offers, such as default settings that aren’t secure, or they might be the consequence of choosing less secure modes of operation