# Automated Binary Analysis
Tags: #binary-reversing 

## Basic Concepts

1. Basic Blocks
2. Control Flow Graph (CFG)
3. Data Flow analysis
4. Control Flow analysis
5. Classic Analysis 
	1. Live Variable Analysis
	1. Available Expression analysis
	1. Reach Definition analysis

## Research Papers
Tags: [[research-papers]]

These are all the important papers with some interesting ideas on reversing and bug hunting.

### Papers

1. [All You Ever Wanted to Know About Dynamic Taint Analysis and Forward Symbolic Execution (but might have been afraid to ask)](https://users.ece.cmu.edu/~aavgerin/papers/Oakland10.pdf)	 ^48f369
2. [Detecting Kernel Memory Disclosure with x86 Emulation and Taint Tracking](https://j00ru.vexillium.org/papers/2018/bochspwn_reloaded.pdf) ^bochspwnreloaded
3. [An Improved Algorithm for Slicing Machine Code - Program Slicing](http://pages.cs.wisc.edu/~venk/papers/oopsla16a.pdf)

## Data flow Analysis

### Live Variable Analysis

- Live variable analysis is a classic data-flow analysis to calculate the variables that are live at each point in the program. A variable is live at some point if it holds a value that may be **needed in the future**, or equivalently if its value may be **read before the next time the variable is written to**.
- Definition : A variable **v** is live at a program point p, if some path from **p** to program exit contains a **r-value (read occurrence)** of **v** which is not preceded by an **l-value(write occurrence)** occurrence of **v**.
- This analysis is a path base specification.
- Liveness flows backwards through the CFG, because the behavior at future nodes determines Liveness at a given node
- ![[Pasted image 20211024144608.png]]
	- GEN[n] - set is referred to r-value
		- Data flow information which is generated within the block **n**
		- it stores the variable which become live as the consequence of the execution of the statement.
	- KILL [n]- the set of variables that are assigned a value in statement (s), KILL (s) is also defined as the set of variables assigned a value in s before any use, but this does not change the solution of the data flow equation):
		- stop the effect of variable from been propagated across a block
		- liveliness of the variable is KILLED. as it may be redefine. (this analysis is not SSA)
		- data flow information become invalid within the block **n**.
	- **definition of v** is refereed to as a **l-value**
	- In[k] - set of variable which are live at the entry of the basic block.
	- Out[k] - set of variable which are live at the exit of the basic block.
- ![[Pasted image 20211024145628.png]]
- Used for :
	- register allocation - the value will be used in the future therefore it needs to be preserved in memory.
	- dead code elimination - if the variable is not going to be used in future in he program then the assignment can be allocation.
- ![[Pasted image 20211026015247.png]]

### Taint Analysis

- Taint tracking can be used to detect info leak vulnerability for example if data structures are copied from kernel to user-land then it can be used to verify if padding bytes within data-structures are been copied of to user-land leaking data. This idea is wonderfully illustrated in [[Program Analysis#^bochspwnreloaded]]
- This concept is also used by angr/symbolic engine to detect bugs.

#### Algorithm
1. [[Program Analysis#^48f369]] paper discussed taint analysis algorithm and how it is implemented in **3. DYNAMIC TAINT ANALYSIS** section.
2. [[Program Analysis#^bochspwnreloaded]] paper has applied taint analysis using [[bochs emulator]] for bug detecting in Windows and Linux kernel.


## Thoughts & Reflections
- 04-11-2021
	- Completed understanding live variable analysis, available expression analysis. These analysis can be easily implemented by using bit vector framework
		- these analysis can be used for implementing constant propagation.
	- I am stuck on lattice concepts and why they are used and important for implementing other optimizations like constant propagation.
	- Nilo said you need to understand [[Reach Definition Analysis]] and [[Live Variable Analysis]] to do all what he is doing. Reach Def analysis can be used to create use-def and def-use analysis.
	- SSA form is model is developed mostly for data flow analysis

## Leaning Material

### Course 1
- [CS 6120: Advanced Compilers: The Self-Guided Online Course](https://www.cs.cornell.edu/courses/cs6120/2020fa/self-guided/)

#### Notes
- This is really good course tried couple of video and they were good.
- There are good practical problem which you have to solve and programming challenge

### Course #2 - Compiler Design and Construction

- Prof Subhajit Roy - IIT Kanpur
- [Program Analysis - Lattice Theory Data flow analysis and Control Flow analysis](https://www.youtube.com/watch?v=QLIQpF9ENqk&list=PLpk1frgfR2WqaacNyPUC-fwUZGMZEY0q9&ab_channel=NPTEL-SpecialLectureSeries)

#### Notes
- This is also very good course and the professor explains it in very easy to understand format
- Good algorithm explanation

### Course #2 - Software Analysis

- [Dr. Mayur Naik](https://www.cis.upenn.edu/~mhnaik/) - Penn State University
- [CIS 547 - Course Page](https://cis547.github.io/)

### Other Courses Options

- [Static program analysis pdf - Anders Moller](https://www.bing.com/videos/search?q=static+program+analysis+pdf&docid=608019841666450092&mid=DB17789C102C04F765CDDB17789C102C04F765CD&view=detail&FORM=VIRE)
	- [PDF for this course](https://cs.au.dk/~amoeller/spa/spa.pdf)
- [CS618-20-21-Course-Material.html](https://www.cse.iitb.ac.in/~uday/courses/cs618-20/cs618-20-21-Course-Material.html)
- [Runtime and Linkers - Discussion 1](https://www.youtube.com/watch?v=pMWpQmaMZsw&list=PLPeEbErKGwN1DpULSX3sQxuU7rkiFpt6d&ab_channel=NPTEL-SpecialLectureSeries)
- [SVF-tools Teaching-Software-Analysis](https://github.com/SVF-tools/Teaching-Software-Analysis)


## Ref

- [CS618 : Program Analysis, IIT Bombay Video](https://www.youtube.com/channel/UCqM55vtIYwe_RyJpOunXpdQ/videos)
- [Static Program Analyses Course - Fedral University](https://www.youtube.com/watch?v=1p1anIHSHzQ&list=PLC-dUCVQghfdu7AG5f_p4oRyKgjDuoAWU&index=20&ab_channel=FernandoMagnoQuintaoPereira)
- [How to write data flow analysis program](http://pages.cs.wisc.edu/~horwitz/CS704-NOTES/2.DATAFLOW.html)
- [CS 6120: Advanced Compilers: The Self-Guided Online Course](https://www.cs.cornell.edu/courses/cs6120/2020fa/self-guided/)
- [Write LLVM dead-store-elimination optimization](https://blog.trailofbits.com/2018/07/06/optimizing-lifted-bitcode-with-dead-store-elimination/)
- [Compiler AI](https://www.youtube.com/channel/UCVnn5LPz8TNgNV5aqlCt6aQ/videos)