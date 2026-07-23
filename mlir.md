# Building a Language Compiler- My experience and learnings.

## Understanding MLIR by Building a Tensor Language Compiler

*July 24, 2026*

MLIR (Multi-Level Intermediate Representation) allows programs to be represented using multiple **dialects**, each describing the program at a different abstraction level. This enables transformations to be performed while preserving semantic information that would otherwise be lost.

To understand this workflow, I implemented a compiler for a small tensor-oriented language using LLVM's MLIR.

---

## Overview

A traditional compilation pipeline:

```
Source Program
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
Lowering to LLVM IR
      │
      ▼
Native Code
```

Rather than directly generating LLVM IR, the compiler first constructs MLIR operations. This intermediate representation retains higher-level information, making transformations prominently easier to express.

---

## Lexical Analysis

The first stage converts the input source into a stream of tokens.

The lexer identifies:

* identifiers
* numeric literals
* operators
* punctuation
* keywords
* function names

---

## Parsing

The parser consumes the token stream and constructs an Abstract Syntax Tree (AST).

The language supports:

* function definitions
* variable declarations
* tensor literals
* arithmetic expressions
* function calls
* return statements

Each grammar rule corresponds to a dedicated AST node, allowing the program structure to be represented independently of any later code generation.

---

## Abstract Syntax Tree

The AST represents the program structure in a hierarchical form.

For example, a function contains:

* prototype
* parameters
* body

Expressions become nodes such as:

* binary expressions
* literals
* variable references
* function invocations

This separation between parsing and code generation keeps the compiler modular and makes later transformations significantly easier.

---

## Generating MLIR

Once the AST is constructed, each node is lowered into MLIR operations.

Instead of emitting LLVM IR immediately, the compiler builds operations inside an MLIR module.

During this phase the implementation creates:

* MLIR context
* module
* functions
* blocks
* operations
* values
* tensor types

The generated IR closely resembles the semantics of the original program while remaining suitable for further transformations.

---

## Verification

After IR generation, MLIR verifies structural correctness.

Verification checks include:

* operation validity
* operand types
* result types
* region structure
* block consistency

This provides **immediate feedback** if the compiler produces invalid IR.

---

## Dialect Lowering

The generated program initially exists in higher-level MLIR dialects.

Compilation proceeds by  lowering operations into lower-level dialects before finally reaching LLVM IR.

Each lowering pass removes abstraction while preserving program semantics.

Instead of performing a single large translation, the compiler applies several smaller transformations, each responsible for one level of abstraction.

---

## Optimization

An important aspect of MLIR is that optimization is performed throughout the lowering process rather than only at the LLVM stage.

Because higher-level information is still available, many transformations are easier to express before lowering to LLVM IR.

The implementation demonstrates how MLIR enables optimization without immediately discarding semantic information.

---

## Running the Compiler

The compiler supports several stages independently.

Examples include:

* printing the AST
* generating MLIR
* verifying generated IR
* running lowering passes
* producing LLVM IR

Being able to inspect every intermediate stage makes debugging substantially easier than treating compilation as a single black box.

---

## Observations

Instead of forcing every optimization onto LLVM IR, MLIR allows transformations to occur while the program still retains high-level structure.


---

## Conclusion


While the language itself is intentionally small, the underlying compiler architecture closely mirrors techniques used in production compiler systems. It demonstrates how MLIR enables structured transformations across multiple abstraction levels instead of relying on a single IR.

---

