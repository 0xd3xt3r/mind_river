

## Bug Class

1. **Root Cause 1: Implicit Specification (ISpec)**. The compiler makes assumptions that conflict with the functionality of the code in respect to the security property. This allows the compiler to perform aggressive optimizations to dismantle the security property.
	1. No-UB, is the most common type of ISpec where compilers assume that the source code is free of Undefined Behavior (UB).
	2. Default-behavior, is the type of ISpec where compilers decide it is appropriate to perform certain default behaviors or make default assump
2. **Root Cause 2: Orthogonal Specification (OSpec)**. The security property exceeds the semantic functionality scope of language specifications. We refer to the theoretical specification required to preserve security during compilation as an Orthogonal Specification (OSpec); they are orthogonal to those of correctness. Such security properties can be the execution time or the lifetime/region of sensitive data.
	1. 