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

Loader translates logical absolute addresses to *physical* addresses when
program is loaded into memory.

With this approach, programs can be loaded to different addresses to start but
cannot be relocated after.

### Execution-Time Bounding
Bounding at execution time, the linker takes object files and produces an
executable object file containing logical addresses for the entire program.
Addressesare translated to physical addresses during execution.

This approach will allow for relocating, but requires special hardware to do
so.

## Address Translation (Partitioning)
During execution time, addresses need to be translated from logical to physical
addresses. 

For each process, we need to store the start of the process memory and the last
address of the process. They are stored in two registers "base" and "limit".

![address-base-limit](./pictures/address-base-limit.png)

Adding the logical address and base value returns the physical address. Logical
address must be checked that it is within the process space (must be less than
limit).

When assigned to the CPU, Memory Management Unit loads the base register, loads
the limit register, and causes an addressing error if the address is not in the
process space.

![address-translate](./pictures/address-translate.png)

## Address Translation (Paging)
Address translation for paging is different, as processes are partitioned into
pages and not contiguous.

For each process, the OS needs to maintain a *page table* for each process. The
page table records which physical frame holds each page.

Instead of relative addresses, programs store virtual addresses:
* page number + page offset (concatenate)
* page number = vaddr / page_size
* page offset = vaddr % page_size

If addresses are 16 bits, and pages are 4096 bytes:
* Use 12 bits (2^12 = 4096) for offset
* The remaining 4 bits (2^4 = 16) represent the max number of pages for any
  process

Translating virtual address 0x3468 (hex)
```
page = vaddr >> 12 = 3 (right shift 12 bits)
```
Look up in the page table, what frame page 3 is stored in
```
page 3 is stored in frame 7
```
Combine frame number with page offset
```
offset = vaddr % 4096
paddr = (frame << 12) | offset)
```

![address-translate-ex.png](./pictures/address-translate-ex.png)


