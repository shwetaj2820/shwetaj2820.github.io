# Building a Language Compiler- My experience and learnings.

I implemented a small compiler for a tensor-oriented language using the LLVM MLIR. The project follows the complete compilation pipeline, starting from parsing source code and progressively lowering it into MLIR before eventually reaching LLVM IR.

## Compiler Architecture

The implementation consists of several stages:

```
Source Code
     │
     ▼
Lexer
     │
     ▼
Parser
     │
     ▼
Abstract Syntax Tree (AST)
     │
     ▼
MLIR Generation
     │
     ▼
Optimization Passes
     │
     ▼
LLVM IR
     │
     ▼
Native Machine Code
```

## Exploring MLIR

One of the most interesting aspects of MLIR is its ability to preserve high-level semantic information throughout most of the compilation process.

Instead of lowering everything directly into LLVM IR, MLIR introduces multiple intermediate representations through **dialects**, allowing compiler passes to operate on abstractions meaningful for problem domains.

During this implementation, I explored concepts such as:

* Lexical analysis and parsing
* Abstract Syntax Tree (AST) construction
* MLIR operations, regions, and blocks
* Lowering high-level constructs into MLIR
* Lowering into LLVM IR
* Compiler optimization passes

Examining the same program across multiple representations provided valuable insight into how compiler transformations are structured and why each intermediate form exists.


