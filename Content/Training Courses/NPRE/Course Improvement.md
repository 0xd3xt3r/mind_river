
# New Ideas

1. Include protocol reversing and explaining for malware like Zeus, [[Mozi]](iot botnet)
2. Include other automation technique like clustering, etc. This will make easy for people want to follow training where they are just executing tools.
3. Write Scanner detector in [[Scapy]], Can you detect what type of network it is scanning(private/public, particular network segment)? Can you identify what all ports it was scanning? Can you deploy this algorithm on stream data or live interface?
4. Include Scapy sniffing labs


# Nullcon 2020

## Training Observations

1. You need to make labs more simple and may be replace the Minetest game with some other simple application which is much simpler.
2. People want learn the concepts that is easy to understand, They want labs on concepts which they can execute during the duration of training. They want to play with the tools and not actually create the tool(not understand things at deeper level).
3. Clarify the term *Reverse Engineering* in the Course proposal.
4. We also need to clarify in the prerequisite section that we will be going to reading of C++ code and we will be writing lot of python code. These are the strong requirement.
5. Also mention in the course proposal that those who want to be prepare about the training in advance and want to progress in the training with light speed they can refer to Scapy Docs and Example and go through [[Minetest]] source code and Docs.
6. Candidates also found difficult to follow the [[Scapy]] framework. We need to
7. Give them more understanding of Protocol flow by showing packet exchange on command-line.
8. Flow of the course needs to be more refined. Refer to [[Attacking Network Protocol]] book. Re-read the flow extract out the flow of the book and repackage the training. Murtuja was right give null session about the training and fix the flow.
9. Lab manual is absolute necessary as there will be people who will not be able to follow the training and they will be frustrated (Aseem was right!). In those cases lab manual will be a good backup option.
10. Write Wireshark dissector so that you can demonstrate the protocol flow in that tool. And also make it clear to the people that you create this just so that it is easy for them to follow. And you not included how to create dissector because then it will be difficult for people to write Lua which no one like.
11. Include Scapy sniffing labs
12. Explain Protocol field size from first 4 layers from the documentation.
13. Provide visual map of course progression. LIke protocol field -> dissection -> fuzzing
14. Do full day lab on fuzzing as people for doing the labs and were very interactive during those labs
15. Network Fuzzing with Radamsa.
16. Explaining the by reversing you are relating network packets and code.
17. Write scanner detection code for Scapy


## Training Reviews

**PitaBit**
I didn't want to interrupt other people during the training, but I think Abhishek point is very important. This course should be reviewed. I think Munawwar had a great knowledge of what he is talking, but **the course is not related with reverse engineering at all**, especially talking about network protocols. Also, **I think it's not well structured**: **a lot of Scapy explanation is missing**, [[Wireshark]] should be used to show what's going on (we are talking about communications) but it's not used... and reading **C++ code is not something expected** for these course, but important if you want to really use some of the concepts explained. I 100% agree that going through the code and understanding the communications of an application is very useful for a lot of things, but not something I would expect with the title and description provided for this training.

**CJHackerz**
In my view even if it was not open source we would have to deal with reading assembly code which would be translate and writing C++ code with any other game (most games are written in c++) and if it was some IoT application it would be written in embedded C (in that scenario you might have got source code from firmware extraction). Yeah not every one can be good at all programming language but any thing going to low level and dealing with deeper layers always requires or it's is expected to have go through source code in white-box or black-box manner. And yeah visually Wireshark would helped but what's is actually going on and how data is process can be only understood by looking into the application logic/code, even I am not expert at C or C++ also same for the [[scapy]] that can only come with practice. And I think Munawwar already showed flow diagrams of all protocol layers which would be visible same in Wireshark under click of mouse. As Munawwar said for the reverse engineering there can be also a entire training session based on just Scapy library and it would go forever but it does have good about of documentation available as well which we can refer to clear doubts about it's features.(edited)

**PitaBit**
I not questioning that it's useful what it's being explained, that I think it is. But, it's not what you would expect by the description of the course. Reading C (and even more this Minetest code) is not really something difficult (a different topic assembly), but again, it's not what the description told us. Looking into code is a white-box approach, but not a reverse engineering of protocols. **The flow diagrams showed were useful, but, in my opinion there was a problem: it's not clear how those diagrams were obtained. I know they are in the code, and I can find them, but I don't think it's the same for everyone, but even more, I would expect that this part was clearly explained**. Also, it's clearly very important the knowledge of Scapy for this training, but it's not even a requirement. And we didn't even go with some basic scripts using it, so everyone can feel comfortable with it. This is a training for network protocol, so I think that's important. And finally, for **some of the labs, we would need to have more knowledge about how the custom classes were built, at least the interface, so we can call some of their properties/methods**. And again, I think Munawwar knows very well what he is talking about, the **problem is how that knowledge is being presented.**

**pitabit**

Reverse engineering is not binary analysis, absolutely agree with that. I'm not talking about binary at all. Reverse engineering can be applied to almost any working thing, indeed the definition you sent in Wikipedia is really good. You said: "**By my understanding Reverse Engineering is recovering the design specification of an application here we are interested in communication specs**", and 100% agree with this, but you didn't "recover" the information, you read it through the code, the wiki for developers or even the schema that the developers put in the C header file. If I'm going through a reverse engineering process to know how a television works (for example), I don't expect to go to the vendor and read the all the design documents, because the main reason to perform a reverse engineering process is that you don't have the documentation you need to understand what you are analyzing. If I have the design documents of the TV, then I don't need "reverse" anything, I just need to understand what I have in front of me. And having the documents doesn't mean that I'm going to understand how the TV works (I also agree with that), this process could be also very complicated, but it's not "reverse". Then, **what would I expect from a reverse engineering protocol? Analyze the traffic packets, see how the packets changes when the application changes, see how the application react when I send packets manipulated...** I know this is incredible challenging, but that's why reverse engineering in protocols (as in the description of the course is mentioned and it's mentioned in Wikipedia) is something not widely developed. I didn't mention any binary or assembly here.

**pitabit**

Finally, the labs are useful, but I think there is a little bit lack of context there. Let me give you a couple of examples: - Lab3 first day, is using Python library twisted. That's not part of standard Python libraries, and it was not explained. I think it could help a lot to add a little bit of context of how the library works. (I know it's not the most complicated thing, but also Wireshark filtering is not and it was very nicely covered in the beginning, so I think makes sense to continue the same learning curve) - Lab1 dissector, the solution is a great way to learn about Scapy and some features, but **getting the solution without more context is almost impossible**. If this is the first lab for Scapy, I would expect to **go first with some examples of bind\_layers, explaining and showing first some examples of the class passed as parameter. But also, the type of the field descriptions was obtained through the documentation. Without going over it first, it would be for us (that you would assume we don't have the knowledge) impossible to guess the type you used in the solution**. I think this course has a lot of potential, but I think there are several details that should be reviewed.

**pitabit**

Confirming my previous feedback (and my opinion posted in discord), very disappointing this training, because there wasn't any communication reverse engineering involved, the main reason for me to join (and indeed the title of the course). Reading code and documentation, it's not reversing. The exercise in the labs could be useful, but the lack of more examples and context of the information there made them very difficult to perform by us, and usually better go directly to the solution provided. Also, **a more engaging experience with lab can be produced when the labs are also done by the instructor, so we can see how someone with knowledge create the solution** (instead of open the solution file already prepared and go over it). The course has potential, the material explained can be very useful, but for a completely different training with a different approach.

**xyz**
The training lacked proper planning and execution. Never in the training, I felt the whole agenda of the exercise was discussed. The material was good however, it went **right into the specifics without giving any clarity on the Big picture**. The 'How' of the course was covered but I felt the **'Why' of the topic was never discussed correctly**. Could have been planned better I felt

**abc**
I wished to learn some of reverse engineering tips and tricks considering the title, except that everything was excellent.



# Ryan Feedback & Requirements

- The lecture will be 3 hours a week.
- Total 36 hours of course.
- The course will run for Three months.
- This will be start from August 2024
- Do python language introduction
- Specify how much time for each module
- Constrains
	- Hours
	- Expertiest
	- people will not be from security