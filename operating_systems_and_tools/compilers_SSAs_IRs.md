(Presented on 2019-12-05)

SSA - (Single Static Assignment) like abstract form of assembly language
  - Instead of assigning things to actual physical registers, it assigns to variables:

        v8 <= add v5, v7
        v9 <= mul v8, v5
    etc

Another thing that is similar is an IR (intermediate representation), which is common in LLVM
  - One of the most common forms of SSA is LLVM's IRs (intermediate representation)
  - IR is like a standardised, instruction set independent, slightly higher level version of
    assembly language

When compiling C code:
C -> Abstract Syntax Tree -> IR (-> IR -> IR) -> Machine code

An IR is basically a control flow graph, so it can use graph theory to help optimise it.
  - The process of turning this control flow graph into a assembly, or into a lower level IR,
    is called "lowering"
  - When you have an unconditional series of instructions, you have a Basic Block (BB). This is
    basically like a box in a control flow graph - or like a box in a GUI disassembler like IDA or
    Ghidra!
  - You can't have a jump into the middle of a basic block - It only has a single point of entry
    at the start (which might be multiple origins), and it will have a single exit point at the
    end (which again might go into different bases, if it's a conditional jump)
  - At the start the of a Basic Block, there will be a "phi node" that describes how a certain
    SSA "variable" has gotten its value
  - "All of this quite well documented in the LLVM documentation"

Basically an IR works like this:

    C / Fortran / Rust / Go
            |
            v
            IR
            |
            v
      x86 / ARM / PPC

  - Clang is the C front end that turns it into IR, and then LLVM takes over (LLVM does "binary
    translation")

GCC apparently has a similar internal representation called "GIMPLE"

wasm is an IR.

The JavaScript compiler in a browser would have one or multiple IRs

