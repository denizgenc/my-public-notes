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
- Input/Output (I/O) devices

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

