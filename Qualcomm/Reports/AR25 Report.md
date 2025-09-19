
## Analytical Skills

> Analyzes, interprets, integrates, and verifies **somewhat complex data and** information from multiple sources to identify root causes and address **somewhat complex** issues.

### Camera Driver Fuzzing with Syzkaller

At the start of the project, the driver exhibited significantly low coverage. Analysis revealed 11 subsystems lacking grammar definitions, which contributed to the reduced coverage—particularly surprising when compared to other drivers. Upon investigating the codebase, the root cause was identified: a substantial portion of the code was triggered through a command pattern involving user-space issued IOCTL commands. Unlike the typical IOCTL interface exposed by the kernel, the camera driver employed an unconventional method using a shared memory space between kernel and user-space. Commands were placed in a buffer by the user-space program, followed by an IOCTL call referencing the shared buffer identifier.

Fuzzing techniques for such drivers were not well established. Through research into open-source projects and in-depth exploration of the Syzkaller codebase, a suitable modeling approach was developed using Syzkaller's language. To enhance coverage, pseudo call support was introduced, which also simplified handling of shared memory buffers. Fuzzing this driver posed additional challenges due to its complex architecture, comprising multiple interdependent subsystems and extensive communication via shared buffers between user-space and the driver.

Here are some highlights of my contributions to the project:

- Added support for 11 new subsystems in the camera which was previously missing.
- Added approximately 1500 lines of grammar.
- Reported 36 CRs (27 medium-rated and 9 low-rated).
- Increased coverage to 13% line coverage and 18% function coverage.
- Crafted 60 manual seed corpus to get better coverage.

While the coverage improvement may seem modest, it is important to note that there are inherent limitations with Syzkaller and its coverage feedback mechanisms.

### FastRPC Training

The FastRPC team requested a security-focused training. After assessing their needs, I designed and delivered a **4-hour workshop** covering:
- **2 hours** on secure coding fundamentals and threat modeling.
- **2 hours** on FastRPC-specific security concerns.

The session was attended by **50–60 engineers** across multiple locations and received positive feedback for its practical relevance and clarity. This initiative showcased my ability to analyze team needs and synthesize complex security concepts into actionable training.

### IoT Security

Many of Android kernel driver was being deployed in IoT products, introducing new security challenges. I led the effort to **port our Android fuzzing harness to IoT platforms**, enabling fuzzing on these constrained environments.

A critical issue emerged: **customers had shell access and could load kernel drivers**, fundamentally undermining the product’s security model. I conducted a thorough threat analysis and highlighted this architectural flaw to stakeholders, prompting a reevaluation of deployment strategies and customer access policies.

This project required integrating technical insights with product-level risk assessment, demonstrating my ability to interpret complex data and identify root causes of systemic security issues.

## Clear Communication

> Communicates **complex** information (e.g., updates, needs, **anticipated** technical issues) in an accurate, appropriate, and timely manner. Leverages different methods of communication to best convey **complex** information based on the audience.

Developed fuzzing process document which goes into the detail of how to capture information during the project execution which server not only as project update page but also as way to detail out the threat model and security research if the same project is picked-up again in the future.
Camera security guidelines.

https://confluence.qualcomm.com/confluence/display/QPSIVD/Product+Security+Assessment+Guidelines

The camera driver involves handling complex buffer operations, which can lead to common pitfalls and mistakes if not properly understood. To help developers navigate these challenges, I’ve documented these issues on our Confluence page. This resource serves as a guide to educate developers about frequently encountered security vulnerabilities in camera driver development.

https://confluence.qualcomm.com/confluence/display/QPSIFT/Security+Assessment+Guidelines+for+Camera+Driver

When designing FastRPC Secure Code training and Camera Driver Assessment guideline I provided the information such that it is addressable to both to engineering team and leads. The information was distilled down to be actionable requests for leadership. Both the trainings included patterns and suggestion on how to approach addressing these problems in development phase.


---

A comprehensive fuzzing process document was developed to capture key information throughout project execution. This document serves not only as a project update resource but also as a detailed reference for threat modeling and security research, ensuring continuity if the project is revisited in the future.

https://confluence.qualcomm.com/confluence/display/QPSIVD/Product+Security+Assessment+Guidelines

Given the complexity of buffer operations in camera drivers, which often lead to common pitfalls and security vulnerabilities, a dedicated Confluence page was created to guide developers. This resource highlights frequently encountered issues and provides actionable insights to improve the security posture during camera driver development.  

https://confluence.qualcomm.com/confluence/display/QPSIVD/Product+Security+Assessment+Guidelines

Additionally, while designing the **FastRPC Secure Code Training** and **Camera Driver Assessment Guidelines**, the content was structured to be accessible and relevant to both engineering teams and leadership. The material was distilled into actionable recommendations, enabling teams to proactively address security concerns during the development phase.

## Coaching & Development

> Participates in personal development (e.g., seeks feedback to improve performance, embraces learning for skill development), **provides support as a project lead and/or people manager** (e.g., **sets clear expectations** for team and/or direct report(s), encourages skills growth, etc.), and aligns individual **and/or team** growth with **department goals**.

"Secure Code Review" training for FastRPC team. I designed and delivered the training on Secure Code review to FastRPC team. The training session was organized for Hyderabad team but we have people who joined remotely from different location like San Diego, China (Automotive Team). There were 2 sessions conducted each of which were 2 hours long.

Developed fuzzing process which goes into the detail of how to capture information during the project execution which server not only as project update page but also as way to detail out the threat model and security research if the same project is picked-up again in the future or by someone else who is working on the similar project. This documentation method is followed by all the member of the team.

https://confluence.qualcomm.com/confluence/display/QPSIVD/Product+Security+Assessment+Guidelines

During the Syzkaller fuzzing project, I delivered a presentation highlighting the intricacies of the Syzkaller language and the complexity involved in shared-memory-based commands, which are commonly found across various drivers. I also introduced the concept of pseudo calls as a technique to enhance code coverage. This knowledge was shared with the broader team to help them apply these strategies in their respective modules, thereby improving both fuzzing effectiveness and overall coverage.

## Collaborative Teamwork
> Collaborates and develops effective working relationships with **key individuals** within and across teams. Demonstrates candor, respect, and fairness

To further strengthen the camera driver security initiative, collaboration was established with the **Camera APT Test Team**, which is responsible for testing the camera stack. The benefits of integrating fuzzing tools into their workflow were discussed, and the team was successfully onboarded to adopt fuzzing as part of their regular testing process. The fuzzing harness developed for the camera driver enables early detection of security vulnerabilities during the software development lifecycle.

Efforts are currently underway to assist the APT team in setting up the fuzzing environment, with plans to train them to independently configure and maintain this setup for future software versions and chipsets. This collaboration has already resulted in the identification and reporting of **four security-related Change Requests (CRs)**.

During the fuzzing campaigns, several Change Requests (CRs) were raised to address identified security issues. To ensure these CRs were properly resolved and verified, I collaborated closely with multiple teams, coordinated the necessary fixes, and oversaw their successful propagation across relevant components.

## Functional Knowledge

> Applies **technical knowledge and skills** in own area of expertise and develops **operational** (e.g., continuous improvements, quality assurance), **financial** (e.g., relevant product details, cost impacts, business case creation), and **organizational knowledge** (e.g., how own role is integrated into the department/organization's framework, interconnectedness of cross-functional teams)

There was a training organized for FastRPC team, I presented in that training and this gave me the opportunity collect feedback about security understanding of the organization and how I need to think from security organization perspective. This help me develop better communication with the team.

Fuzzing Camera driver was very tricky considering its complex communication pattern it uses to communicate with the kernel. To model this type of communication into the fuzzer required me to dig into Syzkaller internals and also digging into unit test cases to figure-out correct sequence of call with appropriate parameters.

In Syzkaller fuzzing project there CR's reported 36 CRs (27 medium-rated and 9 low-rated) if these CR's were reported by external researchers it would have paid 55,800 USD.

## Impactful Innovation

> Develops new and impactful **technical** ideas, approaches, materials, solutions, **and/or procedures** for unique situations. Recognizes ingrained practices and translates new ideas into meaningful action.

Security is sort of consulting jobs and much of my job revolves around suggestions and fixes. To make this thing more scalable and more impactful I document my research into guideline documents for Camera driver security. The guidelines can be consumed by engineering organizations a various levels and this guidelines will help the engineer understand the security problem, with better knowledge the bug can be eliminated from enter the code-base itself.

1. https://confluence.qualcomm.com/confluence/display/QPSIFT/Camera+Driver+Security+Assessment+Guidelines
2. https://github.qualcomm.com/mshelia/syz-fuzz-v2

The biggest bottle-neck for fuzzing writing grammar for Syzkaller, I came across a paper called "SyzDescribe", which would solve this problem. I managed to port this open-source tool which the have provided for llvm-19 and run on the camera driver, the execution time was 8 hours. Unfortunately the results were underwhelming and it didn't result in favorable results.

https://github.qualcomm.com/mshelia/syz_describe

## Strategic Execution

> Completes **moderately complex tasks and/or projects** while meeting quality expectations and adhering to Qualcomm standards of ethical behavior. **Navigates ambiguity across situations** while ensuring deadlines are met.

Out of Band Sub-system is the new feature which is going to push Compute in Enterprise. Since the sub-system exposes the machine to remote attack it becomes very important the conduct a detail security analysis of this sub-system. I initiated project to proactively to address this need. Since it is a very large project we have to prioritize most important component of the sub-system. As part of this project I will be creating threat model, conduct code review and create fuzzing campaign.


## Third Party Feedback Names

1. Camera Driver Fuzzing
	1. [x] Adinarayana Gupta Grandhi
	2. [x] Sukesh Chandra
	3. [-] Veera Venkata Satish Mandru (Camera APT team)
	4. [-] Daksha Gouri Patolia (Camera APT team)
	5. [x] Lingtong Shen (Camera driver and FastRPC Driver Presentation)
	6. [-] Aleksandr Tarasikov
	7. [x] Alok Chauhan
	8. [x] Joey Jiao
	9. [x] Suraj
2. Training Project
	1. [x] Bharath Kumar
	2. [x] Krishnaiah Tadakamalla
	3. [x] Yong Ding
	4. [x] Minghao Xue
	5. [ ] Ping Jiang
3. QPSI
	1. [-] Neeraj
	2. [-] Bernard
	3. [-] Scott Bauer

## Nagaraju Feedback

1. There was a really strange incidents which I am still trying to grapple my mind around it, which is about stealing the credit. The project was developing a fuzzing process we have a details 1 hour call around it and I document details of the that was the process we will be following project. This was the process which I had developed based on my past experience of the dealing with fuzzing projects. He converted the whole thing into slide and presented this to the team as if it was his own work. I was very stocked at this incident and honestly I find it very unethical do such thing to your reports. There are also other various of this where the work is shielded in the name of "we did this". Managers are suppose to give credits to their reports not steal them from some who is two level junior to him. Now this give me the impression any project I do Nagaraju is can keep cashing the credit in meeting in which I might not be part of!
2. Nagaraju misses a very critical skill that any manager should have "listening"! He would often jump to conclusion based on external opinion and double checking with the person about whom the opinion is given. There was one incident where someone gave some figure about Syzkaller fuzzing project and Nagaraju in one-to-one meeting started lecturing me about project not going well. After sharing the project commit history and explaining that because of new process expectations couldn't be meet. He would often asking same questions when I have already discuss it with him.
3. Nagaraju also doesn't care for anyone else's opinion, he would very often bulldoze into meeting with what he wants and that's it! He would pretend that he is listening or considering some other opinion but that only synthetic/tactical empathy which you can it only in words and expression.
4. One-to-One meetings are also very very difficult to get through, and those meeting are only about what he wants me to get done. The would very found be done by very manipulating manner like
	1. Gas-lighting - in this technique, he would often start the meeting me "you have achieved nothing noteworthy this year" once you capitulate to this "done nothing" reality he will show you a path our of this dark cave("done nothing") by what he wants you to do, if you don't follow then you will be forever in the "doing nothing".
		1. The root of this issue is that he doesn't understanding the problem fully before giving criticism. Constructive criticism happens once you understand the full context of the situation and then discuss what will be a reasonable path forward.
		2. It is also very difficult to discuss any problem with him, since he would either deny it or he would turn the situation around and blame you for what is happing and discussion with some very "we have to improve on this", and by "we" he actually mean "me". One example is I asked him for device procurement for Syzkaller fuzzing project, the discussion started with "we need to improve on this Munawwar", you can't even arrange one device, the reality of the situation is "Device Projections" request come to Nagaraju, how am I suppose to scavenge devices from other teams? and why would other teams give their devices to me when they have their own task to accomplish using it.
	2. A Noble Lie - in this technique, he will cite some very senior executive name and he will say he is personally monitoring this project and he has asked him to get so and so done, or else so and so will happened. Now this is a very common technique which I have often encountered in my career and it fine to use it in moderation but doing it too much can reveals the deception.
	3. Direct threat - the technique goes like this, he is the my reporting manager and he controls my performance rating, he is the only person can make or break me, no one else know me in the organization except for what he has to say about me. So, if you want the have a descent life/career in Qualcomm do as he says or consequence will follow in AR.
	4. Undermining the achievement - in numerous instance I would try to discuss the technicality of project and challenges I face but he seems to twist the situation would jump with enthusiasm to push be back with "So what Munawwar? everyone is doing it". I think the objective of this exercise is to beat people into submission. He would often cite that finding CR's is no big deal, the payout for our bug bounty programs is not very big amount, and much bigger cost of fuzzing projects is the salary people draw from Qualcomm. Such arguments leave me speechless and silence is the only way out. I feel very very demotivated with such discussion.
	5. The only way to get through these meetings is to just sit silently and say yes to everything and get out of the room ASAP.
5. I feel emotionally drain working with him, as I have to keep guessing what is the next set of accusation he is going to throw at me.
6. I feel Nagaraju is very micro-managerial, has no empathy, very disrespectful and distrustful toward me and the team.
7. The ideal candidate with whom Nagaraju can be comfortable is someone who is docile and submissive.
8. By writing all this I may have given the impression that he is an evil person, and honestly I don't believe so, as an individual outside the context of Qualcomm, he is a kind person who would listen to you, consider your opinion gather more data about it and form an opinion. As a colleague he is a good person, but as a boss his is a completely different person, exact opposite.
9. Now writing such harsh review can put me in cross-hair and hamper my career in this team and retribution will follow, but this is not only my opinion but lot of people have very similar experience with Nagaraju. Not everyone has the courage to speak-up and someone has to bite the bullet and frankly I have to drag myself everyday to office, to work in such an environment.
10. I often contemplate on the question of what are the motives here? How can you pursue someone to get things done my means of FEAR? who benefits? If we do good job, wouldn't Nagaraju look good in his place? I think the root of this behavior lies in the fact that people will not be able to find job in such "bad market condition" and they have no option but to stick around, but it still doesn't explain this behavior, why?
11. Lack of domain knowledge and technical grasp.
12. Nagaraju seems have access got access to Infinity Gauntlet since his promotion and just like Thanos he has casually flirted with the idea of moving people to different team in team meetings.
13. I have more things to say but I should rest my Pen here!