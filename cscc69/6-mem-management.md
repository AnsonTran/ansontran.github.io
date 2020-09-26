# Memory Management
So far we have looked at CPU scheduling to share (multiplex) the processor.
However, we also need to be able to share main memory as well, since every
active process needs memory. We need to achieve the following things:
1. Support enough active processes to keep CPU busy
2. Use memory efficiently (minimize wasted memory)
3. Keep memory management overhead small


* Relocation
	* Programmers don't know how much or what memory is available when a program
	  actually runs.
	* The scheduler may swap processes in/out of memory, and need to bring it back
	  to a different region of memory. Will need some type of **address
	  translation**.

* Local Organization
	* Machine memory addresses are treated as a one-dimensional array of bytes, while
	  programmers organize code in modules. We will need a way to map between these
	  views.

* Protection
	* A process's memory should not have unwanted accesses by other processes, either
	  intentional or accidental.

* Sharing
	* Processes may have shared memory that they need to access. We will need to
	  specify and control what sharing is allowed.

* Physical Organization
	* Information needs to flow between memory and disk, forming a two-level
	  hierarchy. The CPU only has access to data in registers or memory, not disk.

## Address Binding
Program addresses need to be bound to a memory address before executing.

*symbolic* addresses (variable names) are written by the programmer

*logical* addresses are relative to the start of the stack frame

*physical* addresses are addresses in physical memory that the CPU fetches from
and stores to.

![address-binding](./pictures/address-binding.png)

### Compile-time Bounding
Programs bound at compile time are called *absolute code*, since binary
contains physical addresses.

Compile-time bounding requires that we know what memory the process will use
beforehand. Relocation is not possible, since addresses are already
predetermined in the compiled code.

### Load-time Bounding
Bounding at load time, the compiler translates symbolic addresses to *logical,
relocatable* addresses within compilation unit (source file).

Linker takes object files and translates addresses to logical, absolute
addresses within executable

loader translates logical absolute addresses to *physical* addresses when
program is loaded into memory.

with this approach, programs can be loaded to different addresses to start but
cannot be relocated after.

### Execution-Time Bounding
bounding at execution time, the linker takes object files and produces an
executable object file containing logical addresses for the entire program.
addressesare translated to physical addresses during execution.

this approach will allow for relocating, but requires special hardware to do
so.

