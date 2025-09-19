# LLMV Project

## Basic Concepts

- [SSA Form](https://en.wikipedia.org/wiki/Static_single_assignment_form)
- **Phi Function** - this is the necessary evil of SSA syntax. Since in SSA you can only assign values once its difficult to implement loop structure which is re-assigning variable every time. Other method is loading and storing value from location.
- A basic block can have predecessors and successors. The first basic block of a function is special in the sense that no predecessors are allowed.
- legacy Pass manger and Default Pass manager

## Command

- To make Clang generate the bitcode, you can use the following command:

```shell
$ clang sum.c -emit-llvm -c -o sum.bc
```

To generate the assembly representation, you can use the following command:

```shell
$ clang sum.c -emit-llvm -S -c -o sum.ll
```

You can also assemble the LLVM IR assembly text, which will create a bitcode:

```shell
$ llvm-as sum.ll -o sum.bc
```

To convert from bitcode to IR assembly, which is the opposite, you can use the disassembler:

```shell
$ llvm-dis sum.bc -o sum.ll 
```

The `llvm-extract` tool allows the extraction of IR functions, globals, and also the deletion of globals from the IR module. For instance, extract the `sum` function from `sum.bc` with the following command:

```shell
$ llvm-extract -func=sum sum.bc -o sum-fn.bc
```

Nothing changes between `sum.bc` and `sum-fn.bc` in this particular example since `sum` is already the sole function in this module.

run pass plugin 
`opt --load-pass-plugin=lib/CountIR.so --passes="countir" --disable-output –-stats demo.ll`

## Ref

- https://github.com/banach-space/llvm-tutor
- https://llvm.org/docs/LangRef.html
