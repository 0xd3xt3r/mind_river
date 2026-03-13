---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/vibe-specs"
created-date: 2026-03-11
related:
  - "[[Prompt - Function Signature based on AST]]"
summary: Create accurate function graph and store it in RAG such that LLM can use it for code analysis.
---

## Function Callgraph

### Description

The goal of this project is to generate accurate function call graph. The scope of this project is only to parsing C code. I want to design the data storage for code analysis especially vulnerability analysis.

### Design

1. Use python tree-sitter library to parse c code.
2. You need to parse struct, defs, functions or any global data structure.
3. What data models would you suggest for data flow analysis
4. what data models would you suggest for control flow analysis
5. think of what the data-structure heireachy could be and how can we device an algorithm to derive them?
6. The data will be stored in two format. Graph and table format. For graph format you can use networkx python library and for table format you will store the data in sqlite3 format.
7. In graph data you will store function node and struct node and other primitive data type like int, uint32_t, will also have a node.
8. Each function node will have properties like
	1. function parameter
	2. return data type
	3. start and end line number, file name and the file in which the function was declared.
	4. is it and entry point, the default value of the field should be false. The value will be set during entry-point analysis phase.
	5. if the function parameter and the return data type are of composite data type and the function will have an edge with the struct node.
	6. lock and unlock function
	7. if a function uses some data type locally then it should create a data type edge. with appropriate meta data.
		1. you need to think hard on this and create a resilient algorithm to address this problem.
	8. If the function body make a call via function pointer then we need to work on resolving that function pointer. This is a difficult problem. So we need to address this in multiple phase.
		1. first phase we will create a node with attribute stating that there exist a call. Store the data type of the function pointer and create a edge with the data type definition
		2. in the second phase we will try to resolve the pointer using some algorithm.
			1. one possible approach is to what are all the point at which the pointer is assigned values. The become all the possible value for that variable in the function.
			2. if there is only one assignment that mean that the only value
			3. other approach is context base.
				1. One context could be entry-point. for example in kernel driver there are life cycle method like ioctl interface init/dinit or stateful call and base on the entry-point the life cycle method the value of the pointer is initialized.
				2. for platform driver when the device is attached component binding take place. During this component binding function pointers are initialized.
	9. function should have additional attribute like cyclometric complexity. Think hard on how to create a cyclometric complexity measuring algorithm and apply that to derive the value.
	10. I should also have attributes like memory allocation and free
		1. use a list of function which create memory, like malloc, kmalloc
		2. use a list of function which frees memory. 
	11. it should also record statistics like as shown below, store these attributes in SQL format with function name as key.
		1. loops, nested loops, depth of nested loops
		2. number of array variable,
		3. number of conditional statement like if/else and goto.
		4. number of local variable, number of global variables, number of local struct and number of global structs.
	12. Attributes 
		1. pointer arithmetic operations
		2. memory copy operations like array copy, memory block copy with memcpy, copy_from_user/copy_to_user.
		3. for-loop which is looping an array.
	13. is the function been used for thread operation. like if the function will be execute in different thread context.
9. Each data type node form and edge with other data type or primitive or composite data type.
	1. also store meta data like function pointer
	2. file name
	3. global declaration, local function declaration.
	4. opaque pointers
	5. if the function pointer is been used somewhere then create the edge with the function pointer
	6. one possible field could be pointers, these value can take different value like for 'void \*' can be type-cased to other data type, we have to gather all possible such type and create an edge with those type.
	7. if a data type is used with any function either locally in function scope or as a parameter or globally it will create and edge with the data type node.
10. if the data/function is shielded by if/else def.
11. c language also has typedef this also has to be recorded. The new datatype base on the typedef will also get a node with attribute stating it and also create an edge with the original data type.
12. function pointer type should have a node definition just like function node and it should have similar attributes.
13. There will be some function which will be called but there is not definition for those function. They have to be marked is externally defined.
14. All of this is going to be very complex so design the analysis in different phases such that each phase will enrich the overall data and next phase should take help from previous phase. Think hard on how many phase will we need and what will be different phases? What will be the input and output in current phase and what how the output of previous phase will be in the input of the previous phase. Create and architecture for these different phases. The phase need not only be linear it could also be graph in nature. 
15. Create a Test plan with both happy and failure cases. First create a corpus of C code which cover all possible test case, happy and failure a cases and then create test plans for that test case.

### Acceptance Criteria
- Criterion 1
- 
### Priority
2
### Type
epic

### Assignee  
ross

### Labels
label1, label2

### Dependencies
