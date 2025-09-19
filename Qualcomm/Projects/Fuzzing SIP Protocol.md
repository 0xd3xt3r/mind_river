---
up: "[[Qualcomm Project MoC]]"
aliases:
  - SDP Protocol Fuzzing
  - RCS Protocol Fuzzing
tags:
  - qpsi/project
status: in-progress
created-date: 2025-04-16
completed-date: 2025-04-16
summary: Fuzzign SIP/SDP and RDC Protocol
---

## Objective
> What were you trying to achieve with this project.

There is already some fuzzing work done for SIP/SDP protocol in-context of find header string parsing bugs, but there were some gaps in-terms of fuzzing for other bug classes. Also I want to fuzz RCS protocol which is part of the IMS/3gpp specification.

I plan to fuzzing following protocols
1. [SIP - Session Initiation Protocol](https://datatracker.ietf.org/doc/html/rfc3261)
	1. [Session Mobility](https://datatracker.ietf.org/doc/html/rfc5631)
2. [SDP - Session Description Protocol](https://datatracker.ietf.org/doc/html/rfc2327)
3. [RCS - Rich Communication Services]()
4. [RTP : Real Time Protocol](https://datatracker.ietf.org/doc/html/rfc3550)
5. [RTCP]()

Attack Surface
1. Protocols like RTP usually start being processed after a user has picked up the video call, but **signalling** is performed before the user is notified of the call.
2. Signalling, because it is an attack surface that does not require any user interaction.
3. While RTP is not an interaction-less attack surface because the user usually has to answer the call before RTP traffic is processed, picking up a call is a reasonable action to expect a user to take.
 
---
## Commands
> Commands which you important or used very frequently while working on the project

```bash
# some of the important command in this project
```
---

## Notes
> Random interesting information, observation or anecdotes you observed while working on the projects.

- **Signalling** - The first stage in creating a connection is called signalling. It is the process through which the two peers exchange the information they will need to create a connection, including network addresses, supported codecs and cryptographic keys. Usually, the calling peer sends a call request including information about itself to the receiving peer, and then the receiving peer responds with similar information. SDP is a common protocol for exchanging this information, but it is not always used, and most implementations do not conform to the specification.
1. User Agent Client - these rules govern the construction.
2. User Agent Server - they govern the processing of a request and generating a response.
 3. Protocol Layers
	 1. Lowest Layer - encoding information
	 2. Transport Layer - client sends requests and receives responses and how a server receives requests and sends responses over the network.
	 3. Transaction Layer - handles application-layer retransmissions, matching of responses to requests, and application-layer timeouts.
	 4. Transaction User - TU wishes to send a request, it creates a client transaction instance and passes it the request along with the destination IP address, port, and transport to which to send the request.
---
## Challenges & Opportunities
> While working on a project you might encounter challenges which could potentially be solved to further the project.
- 

---

## Product Docs
> Some of the important internal documents which would you often need to refer very often.

1. [IMSTestBench](https://qwiki.qualcomm.com/qct-bdc/IMSTestBench)
2. 
---
## Build Instruction
> Instruction to clone and build the project from source

```bash
# Cloning instruction

# Building instruction
```

---
## CR Raised
> Some of the bugs which you created while working on the project

### Bug Tracker

| ID | CR | Rating | Date | Comment |
|---|---|---|---|---|
| 1 | 01 | High | 2025-04-16| What happened?|

---

## Thread Model/Attack Surface
> How can we attack the System?

---
## Meeting Notes
> All the important meeting you have done for the project

---
## Event Logs
> A brief timeline of the project

1. 2025-04-16 - Start project discussion

## Tech Team PoC

| PoC Name | Expertise |
|---|---|
|  |  |

---
## Related Research Papers or Articles
> Any research paper or articles which address some the issues which we are facing
- External Research
- Existing IRVR Tickets
---