# Reverse Engineering Basic Terminology

## [[Basic Block]]

- A basic block is a sequence of instructions with a single entry point at its first instruction, and a single exit point at its last instruction.
- when the code jumps to the label that corresponds to a basic block, we know that it will execute all of the instructions in this basic block until the last instruction, which will change the control flow by jumping to another basic block

## Control Flow Graph (CFG)
- CFG models graph relation between [[basic block]] in a function.

## Call Graph
Call Graph model graph relation between call sites and function rather than basic blocks.

## Binary Rewriting
[Paper link](https://link.springer.com/content/pdf/10.1007/s10009-021-00644-w.pdf?pdf=button)

- The goal of binary rewriting is to add, delete, and replace instructions in binary code. There are two main types of binary rewriting techniques: static and dynamic. In static binary rewriting, the binary file is rewritten on disk before the program executes, while in dynamic binary rewriting it is rewritten in memory as the program executes. 
- With **static approaches**, the rewriting process does not incur any overhead during execution as it is performed before the program starts running. However, static binary rewriting is hard to get right: correctly identifying all the code in the program is ultimately reducible to the halting problem in the presence of variable-length instructions and indirect jumps.
- **Dynamic binary rewriting** modifies the code in memory, during program execution. This is typically accomplished by translating one basic block at a time and caching the results, with branch instructions modified to point to already translated code. Since translation is done at runtime, when the instructions are issued and the targets of indirect branches are already resolved, dynamic binary rewriting is not affected by the aforementioned issues for static binary rewriting. However, this style of translation is heavyweight and incurs a large runtime overhead.

### Trampoline Approach
The trampoline then comprises up to six parts:  
1. Preamble (optional): code from O that is to execute before the handler and had to be relocated  
2. Pre-processing: ABI-dependent code to ensure transparency
3. Call to the handler
4. Post-processing: ABI-dependent code to restore the original state  
5. Postamble (optional): code from O that is to execute after the handler and had to be relocated  
6. Jump back to the main instruction stream

## Fundamental Analysis Methods

### Inter-procedural and Intra-Procedural Analysis

#### Flow Sensitivity
A binary analysis can be either flow-sensitive or flow-insensitive.

#### Context Sensitivity

### Control-Flow Analysis
1. Loop Detection
2. Cycle Detection


### Data Flow Analysis

#### Reach Definition Analysis

Which Data definition can reach this point in the program.
1. **Use-def chain** - this tells at each point in program where a variable is used, and where that variable must be defined.
2. **Def-use chain** - tell you where in the program a given data definition may be used.


#### Program Slicing
Slicing is a data-flow analysis that aims to extract all instructions (or, for source-based analysis, lines of code) that contribute to the values of a chosen set of variables at a certain point in the program (called the slicing criterion).