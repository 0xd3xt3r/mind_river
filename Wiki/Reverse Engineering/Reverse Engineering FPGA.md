# FPGA
FPGA stands for **F**ield **P**rogrammable **G**ate **A**rrays. 


## Basic concepts
1. Look up tables(LUT) are the most basic unit of FPGA and the full FPGA is the network of these LUT units.
2. Netlist - 

## Some of the questions I am pursing 
1. Why is reversing FPGA is such a challenge ?
2. How is FPGA development is different from other embedded system development?

## The Forward Engineers Approach

Development tutorial [link](https://alchitry.com/pages/verilog-fpga-tutorials)

Youtube Playlist [link](https://www.youtube.com/watch?v=vjBsywUSKWk&list=PL2935W76vRNGRtB09yXBytO6F3zSZFZGr)


## Thoughts & Reflections

1. FPGS is programming the hardware
2. Understanding of Digital and Combinational logic in important 


## Conversation with Andrew about CPLD and FPGA Reversing

The overall goal of reversing is to convert the bitstream to hardware primitive and then to high level representation

1. What all techniques can we use for reversing?
	1. There are couple of technique that can be use, one of which is [[Fuzzing]], create a corpus of Verilog file and compile it with the tool chain to obtain the bitstream and make a very slight modification to the next file compilation can correlate the result previous compilation the will help you to understand which section of the bitstream file get affected by what change. Execute millions of such cases to map out other section of bitstream. This approach will fail if the bitstream file is encrypted.
	2. visit open FPGA IRC channel and have conversation with other people in open source community about its
	3. As a part of bitstream reversing you have to figure out 
		1. raw memory mapping of device
		2. LUT configuration formation
		3. Bitstream format which stores various configuration 
		4. format in which stores the LUT inter connection configuration
		5. I/O Buffer configuration
2.  What approach can a beginner take to get into this?
	1.  The person should have descent grasp of [[Verilog]] programming and understand what all primitive does the program offer like if/else, function, switch,etc equivalent of higher level programming language like C.
	2.  Then look into chip reference manual and try to understand low level hardware primitive and how does the high level language like [[Verilog]] can be converted to low level hardware primitive.
	3.  Also look into some open-source reversing project and study their existing bitstream format, this will give you idea about what all thing should you be looking for and searching for. Although bitstream format is completed different from vendor to vendor but this will help you understand the primitives of bitstream reversing.
3.  What is the time line in which one should be able to accomplish a project like this?
	1.   Andrew gave example of team at google working on which project where it took them about 2 years to accomplish this.




## Reference
1. http://www.righto.com/2020/09/reverse-engineering-first-fpga-chip.html