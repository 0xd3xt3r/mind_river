---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/knowledge"
status: todo
created-date: 2026-05-14
related:
  - "[[Old Drone Build]]"
summary: Research on the new Drone configuration
---

## Notes

1. You have to design a plan based on your requirements. Following components are selected for long range fights.  
2. **Fixed-Wing / Glider-plane** - these are high efficient plane which low gravity constant and they can glide in the air, which gives them high power efficiency and longer range, There are two options or this:
	1. [Heewing](heewing.com) T1 Range : this is ready to use kit
	2. [Flightory](https://flightory.com/) - 3D Printed Plane Designs : they give you design which you can 3D print it, with full assembly instruction and components list. This might add additional 3D Printer cost.
3. Flight controller - we will control the RC remotely
	1. [Express LRS](https://www.expresslrs.org/) - is the latest open source radio control link. It is also called ELRS.
	2. For frequency, 900 MHz is used for long range(upto 30 km) and 2.4 GHz is used for short range (upto 10 km).
	3. **Radio Transmitter** - this will send and receive signal from the drone.
		1. Radiomaster Pocket M2 ELRS version - it support 2.4 GHz frequency.
			1. The firmware it runs is open source and so people keep hacking in a lot of new feature.
		2. Radiomaster Nomand - this supports both 900 MHz and 2.4 Ghz frequency with Dual Band Gemini tech, this will give us an extended range of 20-40 km range. This is send signal of about 2 watts.
	4. **Receiver** - This module will be installed on the drone which will receive the command and respond back to the controller.
		1. SpeedyBee Nano 2.4 GHz - budget version.
		2. RadioMaster XR2 Nano Multi-Frequency - this supports both 2.4 GHz and 900 MHz.
