# Reverse Engineering Network Protocol 

## Motivation

This subject has create an interest after reading [[Attacking Network Protocols]] and interviewing for Awake Security. It subject is also of interest because it gives opportunity for remote exploitation which has very high impact.

## Introduction

Some of the common things you will see across different protocols are they have some sort of client and server version, compression, encryption and authentication information exchange.

Another interesting thing is how different primitive data-types are used to create complex data representation and how different layers of protocol are put together.

Understanding the data flow of communication is very import part of protocol reversing. This task is easy accomplish by monitoring network as oppose to reversing the binary and trying to relate the communication.

When doing protocol reversing you are trying to accomplish two things, reversing the [[packet structure]] and secondly the [[state machine]] of the client/server.

## [[P2P network]]

1. The resources sent over the network by the master are verified using RSA public key which is embedded in the binary. This is done to protect from distribution of fake/tampered data or configuration file.
2. The bot master have the private key which signs propagates the data over the network.
3. Easy to monitor P2P network and you can enumerate other infected host. Connect to the infected nodes and fetch the address of other infected notes. 
4. One to the popular protocol is [[Kademlia Protocol]]