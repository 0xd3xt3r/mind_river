---
tags:
  - type/writing/social-media
  - training
status: published
created-date: 10-12-2024
---
## Post 14 - Reverse Engineering custom Network Protocol 

🚀 Reverse Engineering custom Network Protocol 🚀  
  
Network Protocol deﬁnes the format and semantics of message exchanged between applications. There are a myriad of proprietary protocols used by application like Skype, Dropbox , etc. they design this protocol to address various goals like bandwidth eﬃciency, encryption, security etc. Since these protocols are designed from scratched and may not be thoroughly review it could potentially have some security vulnerabilities.  
  
Hunting for vulnerability isn't only thing you can do, but having protocol specification can be also used by modern Intrusion Detection Systems(IDS), to do Deep Packet Inspection(DPI) which can enhance its capabilities. IDS earlier relied just based on pattern matching which may produce lots of false positives but deep packet inspection is a whole new ball game.  
  
Custom Protocols are also used Malware and Botnets like Zeus, Emotet, etc to communicate with command centre and by reversing these protocol you can play with these command centre and track their campaigns.