---
tags:
  - type/writing/social-media
  - training
status: published
created-date: 10-12-2024
---
## Post 15 - Protocol Reverse Engineering in Action

💫 Protocol Reverse Engineering in Action 💫  
  
Minetest is an online multiplayer world-building game that uses a custom UDP-based protocol for client-server communication, making it an ideal case study for demonstrating protocol reverse engineering. Because the game is open-source, I was able to explore the code and discovered that some of the protocol specifications were documented in code comments. By reverse engineering the remaining parts, I was able to fully recover the protocol specifications.  
  
Using this information, I developed a basic encoder/decoder in Python with the Scapy framework. With this tool, I successfully authenticated with the server and began navigating the game world.  
  
To illustrate this, I’ve created a demo video (attached to this post). In the demo, you’ll see four bot players controlled by the Python script I mentioned earlier. The script authenticates the bots, processes synchronization data from the server, and sends navigation coordinates that make the bots appear to move in circles.  
  
But that’s not the most interesting part! 🔦  
  
The real intrigue lies in how these bots break game rules 🛠 - like going underground or flying - actions that a normal player wouldn’t be able to perform without special privileges granted by an admin. However, none of these bots have those privileges.  
  
Typically, games have logic to prevent such actions, but if the logic is flawed or only implemented on the client side, it can be bypassed. Since I developed a custom client, these restrictions didn’t apply, showcasing the power of deep protocol understanding.  
  
When dealing with proprietary software, you often don’t have access to protocol documentation, as vendors guard their intellectual property. This can limit your ability to fully understand the software’s capabilities or debug complex, buggy use cases. This training will help you overcome such limitations.  
  
In this training, we’ll dive deep into the reverse engineering process. I’ll walk you through the game’s code and the bot client scripts, along with plenty of lab exercises to reinforce these concepts.

## Post 16 - Layer in Protocol

Although network communication is exchanging bunch of packets back and forth between application these packet are just binary data when the application receives it. 

Protocol data is composed of different layer which is again composed of fields of different size. Different layers are stacked on to each other, each serving a different a specific purpose for example reliability, encoding metadata or for compression/encryption. You can see this pattern in IP Network stack itself, where you have network layer which is responsible for routing the packet to the destination. While, Transport layer ensures the ordering, reliability of the data packet.  

Lets try to understand this in the context of the Minetest Gaming Application, just how the IP Network stack has multiple layer, Application layer will further divided into multiple layer. Layer is nothing but group of fields service some meaning to the objective of the layer. For example Minetest uses UDP protocol to exchange data. Its very common for games to used UDP protocol due its performance advantage but disadvantage of this it is doesn't guarantee reliability and ordering of the data, which can be a problem in-case client game action won't get communicated to the server and not passed on to sub-subsequently to other clients by the server.

Such functional requirement become part of protocol design, the address this problem there is Reliability layer added as part of protocol, then when the application has to transmit reliable data it send it with the layer if not the layer is removed. This kind of flexibility is with gaming applications crave for, the server want to send heart-beat packets or simple time sync packet its send a simple UDP packet without reliability layer and more sensitive data with reliability layer.

When I was reversing the protocol I managed to navigate many intricate protocol design decision which I will be teaching in the Reverse Engineering Workshop. I have attached some sample slides which of the workshop.