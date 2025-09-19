
## Auditing API Calls

1. Auditing a library API is mostly documenting which interfaces expect the caller to perform some security relevant operation before invoking it
	1. Make note of those that are the callers responsibility
2. Audit the code for each of their callers
3. Example of above technique
	1. a library function allocates memory after multiplying the requested size by a constant. Which applications contain callers that pass it an arbitrary untrusted size value?
	2. memcpy expects that n<=sizeof(s1) and n<=sizeof(s2) otherwise an out of bounds read or write will occur
	3. system call expects read, write expect that buf is sized to nbyte
4. Find and document the API functions that require the caller perform a security relevant check 
5. What other insecure patterns can users of this API implement?


## Object Lifecycle Management

> The single responsibility principle states that every module or class should have responsibility over a single part of the functionality provided by the software, and that responsibility should be entirely encapsulated by the class. 
> *Wikipedia*

- Increased code complexity means this will be violated consistently across objects and interfaces
	- Note these violations and where security boundaries are crossed
- Determine what components are responsible for managing the creation and destruction of objects
- These define safe and unsafe ways of declaring variables, objects and structures whose lifetime is explicitly managed by them
- The severity of a vulnerability will depend on the compiler used to compile the code
- This is important to remember when auditing portable code that can run on multiple operating systems and will be compiled by different compilers

## Compiler Differences
- The new operator contains an implicit overflow
	```C
	new int[length]
	malloc(length * sizeof(int))
	```
- Custom new operators and templates often reimplement this pattern
- If the application was developed for an older OS and an older compiler, it may lack the proper runtime memory protections
- Return Value Optimization : 
	- When a function returns a complex data type such as a struct or a class
	- The compiler tries to reduce slow redundant copy operations by creating hidden objects
	- Could result in unexpected behavior if a copy constructor does not execute
```cpp
class C {
public:
	C() {}
	C(const C&) { std::cout << " Copy constructor "; }
};
C d() {
	return C();
}
int main() {
	C o;
	C l(o); // Executes the copy constructor
	C obj = d(); // No copy constructor
}
```

## Vulnerable Code Patterns
- Certain design patterns commonly lend themselves to specific vulnerability patterns
- A function that parses a binary protocol is more likely to contain integer overflows
- A language interpreter virtual machine that processes arbitrary byte code is more likely to contain type confusion vulnerabilities
- An FTP or HTTP server is more likely to contain buffer overflows when handling strings
- The more obscure and complex the vulnerability, the less likely an automated tool can detect it
- For any given piece of data, the more components that operate on it, the more possible states it may be in, the more likely you are to find a bug


## Integer Promotion Rules

![[Pasted image 20240327134838.png]]
![[Pasted image 20240327134900.png]]
![[Pasted image 20240327134913.png]]

## Audit Functions

the following is a brief summary of each row that you can easily refer back to:

| Field | Description|
|---|---|
| Function name | The complete function prototype. |
| Description | A brief description of what the function does. |
| Location | The location of the function definition (file and line number). |
| Cross-references | The locations that call this function definition (files and line numbers). |
| Return value type | The C type that is returned. |
| Return value meaning | The set of return types and the meaning they convey. |
| Error conditions | Conditions that might cause the function to return error values. |
| Erroneous return values | Return values that do not accurately represent the functions result, such as not returning an error value when a failure condition occurs. |

## Ref

1. [How to prepare for a security review](https://blog.trailofbits.com/2018/04/06/how-to-prepare-for-a-security-audit/)
	1. 