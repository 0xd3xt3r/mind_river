---
aliases:
  - NPRE
  - Network Protocol Reverse Engineering
  - Attacking Proprietary Network Protocol
tags:
  - training
---
# Attacking Proprietary Game Protocol

## Overview  

A network protocol deﬁnes the format and semantics of message exchange between applications. In modern times there are a myriad of proprietary application protocols like Skype Protocol, Dropbox Protocol, etc which applications use to achieve various goals like bandwidth eﬃciency, custom encryption/compression, etc. These protocols could have security vulnerabilities. Protocol Reverse Engineering (PRE) is not only useful for oﬀensive purposes but also used by modern Intrusion Detection Systems(IDS), they use the knowledge of protocol speciﬁcation to do Deep Packet Inspection(DPI) which can enhance its capabilities, where it earlier relied just based on pattern matching which may produce lots of false positives. Custom protocols are not only used by legitimate applications but also by malware and botnets like Zeus, Emotet, etc. By reversing malware protocol you can connect to malware servers and track their campaigns.

Protocol Reverse Engineering(PRE) is an art and science of recovering the protocol speciﬁcation of the obscure/proprietary protocol whose documentation is unavailable or poorly documented. There are eﬀorts to develop automated PRE tools but they are largely academic and are not mature enough to be usable, and can't give the accuracy a human analyst can oﬀer. Automated tools face the challenges of heterogeneous protocol data which is often a mixture of text and binary, and it has diﬀerent data types and variable-length ﬁelds and this is the reason I have created this training, it is to help you understand these challenges and learn to recover protocol speciﬁcation. 

This training is divided into two parts, in the ﬁrst part we will learn about Protocol Reverse Engineering principles. We will look at some of the common data formats and other protocol structures and with that understanding we will write a protocol dissector using Scapy framework for a target Desktop game Minetest (open source implementation of Minecraft). Minetest is online multiplayer game in which diﬀerent players can connect to the server and play with other players, there are also many public servers which you can connect and play. Once we have written the decoder we will sniﬀ the connection and look at the communication ﬂow between the client and the server which we will capture and re-analyze the traﬃc to improve the dissector further, using this newly improved dissector we will implement a custom game client/bot which will connect to the server and play as a Bot player.

In the second part, with a decent understanding of the Minetest Protocol we will move on to the oﬀensive side of the training and try to fuzz the game server to ﬁnd some security vulnerabilities, we will start with basic Fuzzer and try to do incremental improvement such that we have good code coverage. Leveraging their reverse-engineered understanding of the protocol, participants will employ Generational Fuzzing by defining the protocol specification in the Boofuzz fuzzing framework and subsequently fuzzing the application. The training will also explore Mutation Fuzzing as an alternative approach to identify potential crashes or vulnerabilities.

This hands-on game hacking training is a takes project-based learning approach, ensuring a comprehensive and practical understanding of Protocol Reverse Engineering. In summary, this training aims to equip you with the knowledge and skills to reverse engineer and understand obscure protocols, enhance IDS capabilities, and explore oﬀensive techniques such as protocol fuzzing to uncover potential security weaknesses. The ultimate goal is to empower participants to create their own tools in the realm of protocol security.


## Why should you take this course?

1. Understand the structure of the requests and response especially useful for a malware analyst.
2. Construction of protocol decoders useful for writing gaming clients or to add support for a third-party proprietary product.
3. Reverse engineer communication of online games like MMORPG which can help you do security testing of multi-player online games.
4. Create Network signatures for malware communication that can be integrated with IDS and IPS, understanding the protocol specification can help you to do deep packet inspection.
5. Write a protocol fuzzer to feed the remote server with crafted randomness in the data to crash the data processing part of the application with the intent of finding security vulnerabilities.
6. Identify security vulnerabilities in protocol implementation like authentication bypass, replay attack, information disclosure, DOS, RCE etc. PRE can also help you to do deeper black-box testing of the application.
7. Build a protocol specification for a vaguely/undocumented protocol.
8. Audit the privacy and security of an application running on your phone/computer by looking at what data is it exporting.

## Who should take this course?

1. DFIR practitioner - to investigation malicious activity in the network
2. Reverse Engineer - write a custom client that fully replicates the existing client software/game.
3. Bug Hunter - Write protocol fuzzer for Black Box testing for application processing remote data, for example, lots of IoT Devices use custom protocol for efficient communication.
4. Malware Analyst - To decode C&C server commands and the data which is exfiltrated
5. Threat Hunting - write network signatures for new emerging APT threats or it could be an intruder in your network, this course will help you decode network and analyze network traffic.
6. Developers - who don't have access to source code or protocol documentation, it usually happens when you are dealing with a legacy system which is too old and the company cannot find any documentation and you intend to migrate the system to new technology.
7. While debugging software over the network, writing a protocol dissector can help you to get a deeper understanding of network communication done by your software.
8. Helps you to do network debugging/diagnostics of application layer data.
9. It helps you understand what is really transmitted over the network.
10. RED Team - take advantage of what the Security Operation Center (SOC) doesn't know. Look for data leaks, do attacks like inject, replay and spoofing.
11. Vulnerability Researcher/Exploit Developer - this will also help exploit developer and vulnerability research to reproduce remote vulnerability and find zero-day bugs.

## Course Outline

## Day 1
1. Networking Basics
2. Capturing Network Traffic
	1. Passive analysis
		1. Network Sniffing
		2. Syscall hooking (strace)
	2. Active analysis
		1. Network Proxies
3. Protocol Reversing
	1. Protocol Structure
		1. Common data format
		2. Data Encoding
		3. Binary Protocol Structure
		4. Text Protocol Structure
	2. Protocol Flow
4. The Recon Agent
	1. Overview of Agentic approach
	2. Convert the passive/active capture workflow into an autonomous reconnaissance agent.
	3. Structured instructions executed by LLM-orchestrated agents with tool-calling capabilities. 
	4. The agent plans → acts → observes → re-plans.
## Day 2
1. Protocol Dissector (targeting Minetest game)
	1. Scapy 101
	2. Implementing protocol dissector in scapy for Minetent game. This section will have Labs on
		1. Protocol decoding TLV format
		2. Packet decompression
		3. Packet Reassembly
2. Custom Client (Bot Player for Minetest Game)
	1. Brief Understanding of Application
	2. Authenticate the client
	3. Establish a valid session
	4. Some game hacks like making the player fly
	5. Create A Bot Army (if time permits)
3. ML-Assisted Traffic Clustering
	1. Pre-classify packets automatically using clustering algorithm
	2. An LLM agent that takes raw PCAP + ML cluster output and automatically generates a Scapy dissector skeleton.
## Day 3
1. Protocol Fuzzing (targeting Minetest game)
	1. What is fuzzing?
	2. Implement Mutation Fuzzer
	3. Implement Dumb Fuzzer
	4. Implement Generation Fuzzing (Protocol Aware Fuzzing)
	5. Creating Harness
2. The Autonomous Fuzzing Agent
	1. The full closed-loop agentic pipeline. 
	2. An LLM agent that reads your Scapy grammar, generates Boofuzz harnesses, runs the fuzzer, monitors coverage, and iteratively improves itself based on feedback.

## Tools of the Trade

Below are some of the tools that you will learn in this training that will make you Protocol Reversing experience more fun.
1. Protocol Reversing tools
	1. Wireshark
	1. Scapy
	2. strace
2. Protocol Fuzzing Tool
	1. Boofuzz (Sulley) fuzzing framework

## Prerequisite

1. Knowledge of security concepts
2. Basic understanding of networking concepts
3. Knowledge of Linux OS
4. Basic Python programming language

## What attendees should bring

1. Laptop with at least 50 GB free space
2. 8+ GB minimum RAM (4+GB for the VM)
3. External USB access (min. 2 USB ports)
4. Administrative privileges on the system
5. Virtualization software – Latest VirtualBox (5.2.X) (including Virtualbox extension pack)
6. Virtualization (Vx-t) option enabled in the BIOS settings for VirtualBox to work

## What attendees will be provided with  

1. Virtual Machine with all the needed software pre-installed.
2. Training Material/slides.
3. Lab Manual
4. What to expect
5. Hands-on Labs
6. The joy of Reverse Engineer (looking under the hood)
7. Getting familiar with Network Protocol Analysis
8. Unlimited Email Support.

## What not to expect

1. This training will **not** make you a Protocol Reversing expert by the end - consider it the beginning of your journey, not the destination.
2. Use the knowledge gained here as a **launchpad** to independently explore both open and closed protocols, deepening your understanding of the underlying design decisions, security assumptions, and implementation flaws.
3. This training does **not** cover exploit development - identifying a crash or vulnerability class is where we stop; weaponizing it is a separate discipline.
4. The agentic AI workflows introduced are **proof-of-concept pipelines**, not production-ready tools — participants are expected to adapt and extend them for their own use cases.
5. This training is **not** a Wireshark or Scapy certification course - tools are taught strictly in the context of protocol reversing, not exhaustively.
6. Prior programming experience in Python is **assumed** - the training will not slow down to teach Python basics.
7. Results from fuzzing during labs are **not guaranteed** - fuzzing is probabilistic; understanding the methodology matters more than finding a bug in class.

## Trainer - Bio

Munawwar Hussain Shelia is currently working as a Vulnerability Researcher at Qualcomm. His primary role involves hunting bug and squashing them before product hits the market. He also develops tools to automate the process of bug detection. With a background Reverse engineering and software development, he possesses a unique perspective on product design which enabling him to eﬀectively identify vulnerabilities. In 2019, he conducted the "Practical IoT Hacking" Training at Nullcon and other conferences. Additionally, he also delivered a talk at the diﬀerent conferences. His areas of expertise include Reverse Engineering, Binary Analysis, Malware Analysis, and Software Exploitation, topics on which he frequently shares insights through his blog, https://taintedbits.com. He has conducted training for various governmental and private organizations worldwide. Notably, he has discovered and reported vulnerabilities in IoT devices and published a paper on Android Malware.

## References

1. [Reverse Engineering Proprietary Protocols - OWASP New Zealand Day 2025](https://appsec.org.nz/conference/training-reverse_engineering_proprietary_protocols.html)
2. [Reverse-Engineering and Fuzzing Custom Network Protocol - Blueteam Con Chicago, USA 2024](https://blueteamcon.com/directory/reverse-engineering-and-fuzzing-custom-network-protocol/)
3. [Reverse-Engineering and Fuzzing Custom Network Protocol - Nullcon Goa 2023](https://web.archive.org/web/20230628082912/https://nullcon.net/goa-2023/training/reverse-engineering-and-fuzzing-protocol/)
4. Reverse-Engineering and Fuzzing Custom Network Protocol - Nullcon Goa 2020
5. [Firmware Reverse Engineering - a Pragmatic Approach (Besides Delhi, 2020)](https://bsidesdelhi.in/speakers.php)
6. [Reverse Engineering Bare-Metal IoT Firmwares - Moving beyond Linux (c0c0n Conference, 2020) ](https://india.c0c0n.org/2020/agenda)
7. [Practical IoT Hacking Training, Nullcon Goa 2020](https://web.archive.org/web/20201203003925/https://nullcon.net/website/goa-2020/training/practical-iot-hacking.php)
8. [IoT Hacking workshop (1-Day, CPX 360)](https://www.checkpoint.com/cpx/asia/)
9. [HITB Lab - Writing Bare-Metal ARM Shellcode](https://web.archive.org/web/20211016153622/https://cyberweek.ae/2020/writing-bare-metal-arm-shellcode/)


# Protocol Reverse Engineering, an Agentic Approach.

## Description 
A network protocol defines the format and semantics of message exchange between applications. In modern times, there are a myriad of proprietary application protocols — like the Skype Protocol, Dropbox Protocol, etc. — which applications use to achieve various goals such as bandwidth efficiency, custom encryption/compression, and more. These protocols could contain security vulnerabilities. Protocol Reverse Engineering (PRE) is not only useful for offensive purposes but is also used by modern Intrusion Detection Systems (IDS); they leverage knowledge of protocol specifications to perform Deep Packet Inspection (DPI), which enhances detection capabilities beyond the pattern matching approaches that often produce high false-positive rates. Custom protocols are not only used by legitimate applications but also by malware and botnets like Zeus, Emotet, etc. By reversing a malware protocol, you can connect to malware servers and track their campaigns.

Protocol Reverse Engineering (PRE) is the art and science of recovering the protocol specification of an obscure or proprietary protocol whose documentation is unavailable or poorly documented. There are ongoing efforts to develop automated PRE tools, but they are largely academic and not mature enough to be practically usable, and cannot yet match the accuracy a skilled human analyst can offer. Automated tools face the challenges of heterogeneous protocol data, which is often a mixture of text and binary, with different data types and variable-length fields. It is for this reason that this training was created — to help you understand these challenges and learn to recover protocol specifications effectively. The training also introduces modern machine learning clustering techniques, enabling participants to automatically group and classify unknown message types at scale, dramatically reducing manual analysis time. Finally, participants will explore agentic AI workflows that orchestrate the entire reverse engineering pipeline autonomously, from traffic capture through dissector generation to fuzzing — representing the cutting edge of the field.

This training is divided into two parts. In the first part, we will learn about Protocol Reverse Engineering principles. We will examine common data formats and other protocol structures, and with that understanding we will write a protocol dissector using the Scapy framework targeting a desktop game — Minetest, an open-source implementation of Minecraft. Minetest is an online multiplayer game in which different players can connect to a server and play together; there are also many public servers available to connect to. Once we have written the decoder, we will sniff the connection and analyze the communication flow between the client and the server. This captured traffic will be re-analyzed to improve the dissector further. Using this improved dissector, we will implement a custom game client/bot that connects to the server and plays as a bot player. We will also apply unsupervised machine learning clustering (DBSCAN) to automatically infer message types from raw traffic, bridging the gap between manual analysis and automated tooling.

In the second part, with a solid understanding of the Minetest protocol, we will move to the offensive side of the training and attempt to fuzz the game server to discover security vulnerabilities. We will start with a basic fuzzer and incrementally improve it to achieve better code coverage. Leveraging their reverse-engineered understanding of the protocol, participants will employ Generational Fuzzing by defining the protocol specification in the Boofuzz fuzzing framework and subsequently fuzzing the application. The training will also explore Mutation Fuzzing as an alternative approach to identify potential crashes or vulnerabilities. Each day's learning culminates in an agentic AI capstone, where participants will convert their manual workflows — traffic capture, protocol clustering, and fuzzing — into autonomous, LLM-orchestrated agent pipelines capable of operating with minimal human intervention.

This hands-on, game-hacking training takes a project-based learning approach, ensuring a comprehensive and practical understanding of Protocol Reverse Engineering. In summary, this training aims to equip you with the knowledge and skills to reverse engineer and understand obscure protocols, enhance IDS capabilities, apply machine learning to automate protocol inference, and explore offensive techniques such as protocol fuzzing to uncover potential security weaknesses. The ultimate goal is to empower participants to create their own tools — and their own agents — in the realm of protocol security.

## Why Should You Take This Course?

1. Understand the structure of protocol requests and responses — especially useful for malware analysts decoding C2 communication.
2. Construct protocol decoders useful for writing game clients or adding support for third-party proprietary products.
3. Reverse engineer the communication of online games like MMORPGs, enabling security testing of multiplayer online game servers.
4. Create network signatures for malware communication that can be integrated with IDS and IPS — understanding the protocol specification enables Deep Packet Inspection far beyond simple pattern matching.
5. Write a protocol fuzzer to feed a remote server with crafted, randomized data to crash the data-processing component of the application, with the intent of finding security vulnerabilities.
6. Identify security vulnerabilities in protocol implementations such as authentication bypass, replay attacks, information disclosure, DoS, and RCE. PRE can also enable deeper black-box testing of an application.
7. Build a protocol specification for a vaguely or undocumented protocol.
8. Audit the privacy and security of an application running on your phone or computer by analyzing what data it is exfiltrating over the network.
9. Apply unsupervised machine learning clustering to automatically classify and group unknown message types from raw traffic captures, reducing manual analysis time from days to hours.
10. Design and deploy agentic AI workflows that autonomously orchestrate the entire reverse engineering pipeline — from traffic capture and dissection through to crash triage and vulnerability reporting.


## New content

1. Automated Triage & Clustering (Replacing raw manual analysis) - Capturing traffic and using ML/Entropy-based clustering to identify message types.
2. Binary-Assisted Inference (New) - Tying network traffic to the binary. Using tools (radare2/Frida) to watch the parser slice the network buffer and map field boundaries.
3. Dynamic Probing & Refinement (Upgraded) - Writing the Scapy dissector, but validating it via active probing and state-machine manipulation.
4. Structure-Aware & Coverage-Guided Fuzzing (Upgraded) - Moving beyond Boofuzz to combine protocol specs with binary instrumentation (AFLNet) for deep vulnerability discovery.


## New Changes

#### Day 1 Agentic Capstone — "The Recon Agent"

Concept: Convert the passive/active capture workflow into an autonomous reconnaissance agent.

Theory: What makes this agentic?

Agentic workflows = structured instructions executed by LLM-orchestrated agents with tool-calling capabilities. The agent plans → acts → observes → re-plans.
Architecture walkthrough:

```bash

User Prompt: "Capture and profile all Minetest UDP traffic for 60 seconds"
       │
       ▼
 [Planner Agent]  ──→  Generates sub-tasks:
       │               1. Spawn tcpdump tool
       │               2. Run strace on Minetest PID
       │               3. Aggregate PCAP + syscall logs
       ▼
 [Capture Tool]   ──→  Executes tcpdump / strace
       │
       ▼
 [Summary Agent]  ──→  Produces structured report:
                        - Unique destination IPs/ports
                        - Packet size distribution
                        - Estimated message types
```

- **Lab 1-A (Agentic)**: Wire a simple LangChain/LangGraph agent that:
	- Uses a run_tcpdump tool to capture traffic for N seconds
	- Passes the PCAP to a pcap_summarizer tool
	- Returns a structured JSON summary of all observed packet flows — automatically 

#### Day 2 - ML-Assisted Traffic Clustering (Pre-Dissector Step)

*Before writing any Scapy code, use ML to pre-classify packets automatically.*

- Feature Extraction: Packet length, byte entropy, first-N-bytes fingerprint, inter-arrival time
- Unsupervised Clustering: Apply DBSCAN and K-Means to group packets into likely message types
- Sequence Alignment: Use Needleman-Wunsch alignment to find static fields (opcode bytes) vs dynamic fields (coordinates, timestamps) across a cluster
- Visualization: PCA / t-SNE 2D plots showing how message types separate
- **Lab 3-A**: Feed 500+ Minetest packets into a Python clustering pipeline → auto-identify CHAT, MOVEMENT, BLOCK_UPDATE clusters without reading the source code

### Day 2 Agentic Capstone — "The Dissector Builder Agent"

Concept: An LLM agent that takes raw PCAP + ML cluster output and automatically generates a Scapy dissector skeleton.

Architecture walkthrough:
```bash

Input: PCAP file + cluster labels from Day's ML pipeline
       │
       ▼
 [Field Inference Agent]
   - Reads cluster centroid byte patterns
   - Feeds hex chunks + context to LLM:
     "This is a 4-byte field at offset 2, value is always 0x00-0x09.
      Game context: voxel game. What is this likely?"
   - LLM returns: "Opcode / Command ID"
       │
       ▼
 [Scapy Code Gen Agent]
   - Generates Python Scapy class skeleton for each cluster
   - Names fields based on LLM semantic inference
   - Writes to dissector.py
       │
       ▼
 [Validation Agent]
   - Runs Frida hook on Minetest binary
   - Compares runtime field boundaries to generated dissector
   - Reports mismatches → feeds back to Field Inference Agent
```
- **Lab 2-A (Agentic)**: Build a multi-step agent that:
	1. Consumes clustered PCAP data
	2. Prompts an LLM per cluster to infer field semantics
	3. Outputs a ready-to-refine Scapy dissector file
	4. Validates it against live Frida traces 🔄

### Day 3 Agentic Capstone — "The Autonomous Fuzzing Agent"

Concept: The full closed-loop agentic pipeline — the crown jewel of the course. An LLM agent that reads your Scapy grammar, generates Boofuzz harnesses, runs the fuzzer, monitors coverage, and iteratively improves itself based on feedback.

Architecture walkthrough:

```bash
                    ┌────────────────────────────────────┐
                    │         ORCHESTRATOR AGENT         │
                    │   (Plans, delegates, re-evaluates) │
                    └───────────────┬────────────────────┘
                                    │
          ┌─────────────────────────┼─────────────────────────┐
          ▼                         ▼                         ▼
  [Grammar Agent]          [Harness Gen Agent]        [Coverage Agent]
  Reads Scapy dissector    Writes Boofuzz session     Runs Frida script
  + Minetest binary CFG    + seed corpus from         Reports new block
  Generates fuzzing        bot session logs           hits per input
  grammar JSON                     │                         │
          │                        │                         │
          └────────────────────────┴─────────────────────────┘
                                   │
                          ┌────────▼─────────┐
                          │  Feedback Loop   │
                          │                  │
                          │ Coverage dropped?│──→ Re-prompt Grammar Agent
                          │ Crash found?     │──→ Triage Agent: symbolicate,
                          │                  │    classify crash type
                          │ Stale coverage?  │──→ Seed Mutator Agent:
                          │                  │    generate new seed variants
                          └──────────────────┘

```
Key Agentic Patterns taught:

| Pattern                  | Application in NPRE                                                       |
| ------------------------ | ------------------------------------------------------------------------- |
| **ReAct** (Reason + Act) | Agent reasons about which protocol field to target next based on coverage |
| **Tool Use**             | Fuzzer, Frida, crash symbolizer as tools                                  |
| **Reflection**           | Agent reviews its own generated grammar for completeness                  |
| **Multi-Agent**          | Separate agents for grammar, harness, coverage, triage                    |
| **Feedback Loop**        | Coverage + execution stats re-prompt agents iteratively                   |

**Lab 3-A (Agentic - Grand Finale):** Students build and run the full pipeline end-to-end against the Minetest server:

1. 🧬 Grammar Agent generates a Boofuzz grammar from the Day 2 dissector
2. 🔧 Harness Agent writes the fuzzing session code
3. 📊 Coverage Agent instruments Minetest via Frida
4. 🔄 Orchestrator monitors coverage, re-prompts when stuck
5. 🐛 Triage Agent classifies any crashes found

#### What will they learn?

1. Capture and intercept live protocol traffic using professional tools
2. Decode binary protocols manually from raw hexadecimal packet dumps
3. Write production-ready Scapy dissectors for unknown binary protocols
4. Hook running processes to extract pre-encryption network buffers live
5. Build a fully working custom client against a real server
6. Fuzz live network services using mutation and generation techniques
7. Cluster unknown traffic automatically using unsupervised machine learning algorithms
8. Wire manual RE workflows into autonomous LangChain agent pipelines
9. Define protocol grammars in Boofuzz for structured protocol-aware fuzzing
10. Triage fuzzer crashes and map them to exploitable vulnerability classes