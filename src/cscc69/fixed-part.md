# Fixed Partitioning
Assuming entire address space of process must be in main memory and the process
uses a contiguous chunk of memory, we need to figure out how we can partiiton
the main memory for each process.

* The number of active processes is limited by the number of partitions
* We may get *internal fragmentation*, where a process is smaller than the
  partition, and memory is wasted.
* Additionally, programmers must deal with programs that exceed the partition
  size (*overlays*)

## Equal-sized Partitions
In equal-sized fixed partitioning, we divide memory into equal sized regions.
For example, if we have 32MB memory, we may divide into 4 chunks of 8MB each.

Processes can load into any available partition.

## Unequal-sized Partitions
We may decide to partition our main memory into different sizes to accomodate
for different memory sizes of processes. For example, 32MB of memory maybe
divided into 2 partitions of 4MB each, 1 partition of 8MB, and 1 partition of
16MB.

We can queue up processes for available partitions in two ways: 

Queue per partition: We have a queue of processes for each partition, and
assign a process to the smallest size partition it can fit. In this policy, if
there are many small size processes, a larger partition may go unused as all
the processes queue up for the smaller partitions.

Single queue: We assign processes to the smallest *available* partition. In
this policy, processes will fill up all the partitions as they are available.
