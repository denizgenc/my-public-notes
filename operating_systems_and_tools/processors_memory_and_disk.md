Processors
----------
What are processors?

- processors have a bunch of registers, like areas on your desk. r\_00-> r\_16
- add r0, r7 r13 (add r0 and r7, put result into r13)
- fetch 0x12398af... r7 (put something from memory at address 0x... into r7)
- etc
- basically telling a stupid robot to do things - move numbers from one place to the other

Processors have 2 different instruction sets

- Instructions for boring computer stuff (mov, add, etc) - user space stuff
- Restricted instructions, for privileged access - setting up page tables, accessing devices,
  etc

The OS' job is to take a bunch of processes that should be unprivileged, switch between them, and do
all the privileged stuff for them.

No such thing as concurrency on a single thread, single core processor - just "cheating".

- Time-sharing operating systems. (The first ones didn't really take security really seriously -
  Unix did, but it didn't take all the implications of all the things people can do nowadays
  into account)

Memory
------
Each successive thing is slower than the previous:  
Registers > Cache > Memory > Disk (> Network)   
However the amount of bytes you can have increases and the price per byte decreases too.

The cache is "invisible" to your CPU - you can only address registers or memory addresses. It's the
job of (some other part of) the CPU to cache frequently used parts of memory.

Each memory address (0x123456789...) can store a byte - it's "Byte Addressable".

Do modern 64-bit PCs have 2^64 bytes of memory? No, we have much less, so we can't address all of
it. We have to use virtual memory.

The page table maps addresses from virtual memory to physical memory. It's called the page table
because the units of memory are called "pages" (usually 4kB, sometimes 2MB). The page table itself
is stored in memory.

- There are several different page tables, one for each process. They're switched out as the OS
  time shares ("context switches") between processes
- Only parts of the page tables are switched out though - parts of the page table remain (for
  example, the parts that map to where operating system calls stay where they are).
- The thing that does all this is the Memory Management Unit (MMU). Nowadays this is integrated into
  the CPU, but it wasn't always.

How do you get data from disk?
-----------------------------
Syscalls! You hand over the processor to the operating system, it does _privileged stuff_ and then
it hands back a file descriptor (a number that relates to the specific file you're trying to
access).

**How does it get the bytes from the file in the first place though?**

You give it a pointer to a bit of your memory and it throws a few bytes in there, until you request
the next amount of bytes.

**But what if you have a really big file?** 

Memory mapping (MMAP). The OS will map a file to an entire region of virtual memory. If you don't
have enough memory, it will try to map as much as possible to virtual memory. If you then try to
access a part of the file that isn't mapped, this will cause a page fault, and the OS will catch it,
load that part into physical memory, and then map that to virtual memory.

Misc.
------------------
When the OS starts, it sets up the processor so that it understands that "syscall xxx" refers to
a specific part of memory.

Unix is great because all the devices (under `/dev/`) are files! So you don't have to write a
syscall for each new device you want to fiddle with - you just use the OS file read/write things
(and file descriptors, etc).

Real time systems make guarantees of "you will have x amount of time to execute before you get
context switched" - so the process can make sure it will execute in that amount of time (think a
critical process, like a flight computer, that you wouldn't want to be switched away from just
before it does a vital calculation)

- However what happens if there's a bug in the process, and it enters an infinite loop? That's
  really bad and it breaks real time systems, basically. Nothing's perfect :)
