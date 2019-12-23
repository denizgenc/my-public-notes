# the x86 Architecture
(Source: page 8-9 of _Art of Assembly Language, 2nd Edition_ by Randall Hyde)


## The Von Neumann Architecture
The x86 architecture (and presumably most modern CPU/computer architectures?) is classified as a
Von Neumann Architecture Machine (Wikipedia link:
https://en.wikipedia.org/wiki/Von_Neumann_architecture).

### Basic components and the system bus
This architecture has three main components:
- A CPU
- Memory
- Input/Output (I/O) devices (e.g. storage, keyboard, display)

These components are all connected together by the system bus. The system bus is compromised of:
- The address bus
- The data bus
- The control bus

The buses are used as follows:
1. When the CPU needs to read/write data in memory or an I/O device, it places a numeric value on
   the address bus - every memory location and I/O device ports have a related "address", which is
   the value written to the address bus.
2. The data bus is used for the transfer of data between the components
3. The control bus controls the direction of the flow of data (e.g. Memory -> CPU vs. CPU -> Memory)

### Alternatives to Von Neumann
The main alternative to the Von Neumann architecture is the Harvard Architecture - the main
difference being that in Harvard, instructions and data have separate memory and pathways to access.
This has the advantage that you can't treat instructions as data and data as instructions (safety).
The disadvantage is that you can't treat instructions as data and data as instructions (speed and
the feelings of LISP wizards).

## Registers
A register on a processor is a very small store of data that the processor can "directly" access. It
is usually an order of magnitude faster than memory access.

Almost all calculations on a x86 CPU involve a register - they are a "middleman in nearly every
calculation".

The x86 architecture has 4 different types of registers - general-purpose, special purpose
application-accessible, segment, and special-purpose kernel-mode registers.

Segment registers traditionally contained values that represented addresses in memory that
correspond to the stack, the code, and the data. They aren't used much in modern OSes, so these
notes won't cover them. (The reason for this is that modern OSes use a memory model that sets nearly
all segment registers to the same value, and uses paging to manage the different location of data in
memory instead).

Also, the kernel-mode registers are for writing low-level tools, like operating systems or
debuggers - these aren't in the scope of the book I'm sourcing these notes from so they won't be
included.

### General-purpose registers
(Note: This section has been adapted from the book to be more relevant to the x86-64 architecture.
Sources used for this information includes the [x86 Assembly
Wikibook](https://en.wikibooks.org/wiki/X86_Assembly/X86_Architecture) and the [AMD64 Architecture
Programmer's Manual, Volume 3: General-Purpose and System
Instructions](https://www.amd.com/system/files/TechDocs/24594.pdf), specifically the "Registers"
section from page xxix onward.)

The x86 architecture provides 8 general-purpose registers (GPRs). These are:
- AX (accumulator, used in arithmetic operations)
- CX (counter, used in loops)
- DX (data, used in arithmetic and I/O opersations)
- BX (base, used as a pointer to data)
- SP (stack pointer, points to the top of the stack)
- BP (stack base pointer, pointes to the bottom of the stack)
- SI (source index, used to point to source in stream operations)
- DI (destination index, used to point to destination in stream operations)

All of these registers can be accessed in 16-, 32-, and 64-bit modes.
- Append E to the above register names to represent the register in 32-bit mode (EAX, ECX, EDX...)
- Append R to the above register names for 64-bit (RAX, RCX, RDX, RSP, RBP...)
- 8-bit access is also available for various registers, in different ways. Most importantly,
  registers {A,B,C,D}X have their upper and lower 8 bits accessible using H instead of X for the
  high half, and L for the low half (e.g. AH, CL, DL...)

Along with the above registers, x86-64 introduced 8 additional GPRs: R8, R9, R10 ... R15. These also
have different modes:
- Rn is the 64-bit mode
- RnD is the 32-bit mode, using the lowermost 32 bits (D for doubleword, 4 bytes = 32
  bits)
- RnW is the 16-bit mode, using the lowermost 16 bits (W for word, 2 bytes = 16 bits)
- RnB is the 8-bit mode, using the lowermost 8 bits

Note that these are _modes_ - RAX, EAX, AX, AH and AL are all on the same register, so modifying RAX
will modify EAX etc. (So there are 16 general-purpose registers on an x86-64 processor in total.)

Finally, note that the name "General-purpose register" is somewhat of a misnomer.  SP (the stack
pointer) and SB (the stack base) point to specific addresses in the stack, which is where important
data about the current process is stored. Using these registers for general calculations will mess
things up big time.

### The FLAGS register
In the processor there is a 16-bit register called the FLAGS register, that indicates the status of
the processor. Most of the one bit flags are exclusively for use by the kernel, or of very little
interest to an application programmer, but there are a few that are very important:
- 0: the carry flag (set if the last arithmetic operation carried/borrowed (depending on addition or
  subtraction) a bit beyond the size of the register
- 6: the zero flag (set if the result of an operation is zero)
- 7: the sign flag (set if the result of an operation is negative)
- 11: the overflow flag (set if a signed arithmetic operation resulted in a value too large for the
  register to contain - when two positive signed values result in a negative, or vice versa)

These four flags can be called _condition codes_, and they allow the application to be aware of the
result of previous computations (especially important for conditional parts of our code, i.e. the
assembly equivalent of if, for and while statements).

Also of interest are flags 10 (direction), 9 (interrupt disable), 4 (adjust/auxilliary carry) and 2
(parity).

(Note that the FLAGS register on x86-64 is actually 64 bits wide, with the name RFLAGS \[and on
32-bit architectures it was EFLAGS\]. However, most of the flags beyond the first 16 are not of
great interest \[to beginners?\] and in fact the flags from number 22 onwards are all reserved for
future use, so should not be used.)

### Other registers
There are other registers in 64 bit processors, with even larger sizes. These include:
- SSE registers (128-bit)
- AVX registers (256-bit)
- AVX-512 registers (512-bit)

These are all extensions to the x86 architecture, and are all related to floating point
calculations. I won't cover them in great detail, because unfortunately my main source (Hyde)
doesn't really cover them.
