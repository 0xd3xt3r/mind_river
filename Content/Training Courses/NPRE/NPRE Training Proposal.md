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
4. Protocol Dissector (targeting Minetest game)
	1. Scapy 101
	2. Implementing protocol dissector in scapy for Minetent game. This section will have Labs on
		1. Protocol decoding TLV format
		2. Packet decompression
		3. Packet Reassembly
5. Custom Client (Bot Player for Minetest Game)
	1. Brief Understanding of Application
	2. Authenticate the client
	3. Establish a valid session
	4. Some game hacks like making the player fly
	5. Create A Bot Army (if time permits)
6. Protocol Fuzzing (targeting Minetest game)
	1. What is fuzzing?
	2. Implement Mutation Fuzzer
	3. Implement Dumb Fuzzer
	4. Implement Generation Fuzzing (Protocol Aware Fuzzing)
	5. Creating Harness

## Tools of the Trade

Below are some of the tools that you will learn in this training that will make you Protocol Reversing experience more fun.
1. Protocol Reversing tools
	1. Wireshark
	1. Scapy
	2. strace
	3. scapy
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

1. Become a Protocol Reversing Ninja.
2. Use the knowledge gained in this training to start exploring some Open and Close Protocol to improve your understanding of this topic. That will help you to get a deeper understanding of some underlying issues more closely.

## Trainer - Bio

Munawwar Hussain Shelia is currently working as a Vulnerability Researcher at Qualcomm. His primary role involves hunting bug and squashing them before product hits the market. He also develops tools to automate the process of bug detection. With a background Reverse engineeering and software development, he possesses a unique perspective on product design which enabling him to eﬀectively identify vulnerabilities. In 2019, he conducted the "Practical IoT Hacking" Training at Nullcon and other conferences. Additionally, he also delivered a talk at the diﬀerent conferences. His areas of expertise include Reverse Engineering, Binary Analysis, Malware Analysis, and Software Exploitation, topics on which he frequently shares insights through his blog, https://taintedbits.com. He has conducted training for various governmental and private organizations worldwide. Notably, he has discovered and reported vulnerabilities in IoT devices and published a paper on Android Malware.

## References

1. [Reverse-Engineering and Fuzzing Custom Network Protocol - Blueteam Con Chicago, USA 2024](https://blueteamcon.com/directory/reverse-engineering-and-fuzzing-custom-network-protocol/)
2. [Reverse-Engineering and Fuzzing Custom Network Protocol - Nullcon Goa 2023](https://web.archive.org/web/20230628082912/https://nullcon.net/goa-2023/training/reverse-engineering-and-fuzzing-protocol/)
3. Reverse-Engineering and Fuzzing Custom Network Protocol - Nullcon Goa 2020
4. [Firmware Reverse Engineering - a Pragmatic Approach (Besides Delhi, 2020)](https://bsidesdelhi.in/speakers.php)
5. [Reverse Engineering Bare-Metal IoT Firmwares - Moving beyond Linux (c0c0n Conference, 2020) ](https://india.c0c0n.org/2020/agenda)
6. [Practical IoT Hacking Training, Nullcon Goa 2020](https://web.archive.org/web/20201203003925/https://nullcon.net/website/goa-2020/training/practical-iot-hacking.php)
7. [IoT Hacking workshop (1-Day, CPX 360)](https://www.checkpoint.com/cpx/asia/)
8. [HITB Lab - Writing Bare-Metal ARM Shellcode](https://web.archive.org/web/20211016153622/https://cyberweek.ae/2020/writing-bare-metal-arm-shellcode/)

