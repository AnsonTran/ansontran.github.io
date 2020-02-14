# Non-preemptive Scheduling
In non-preemptive scheduling, once th CPU is allocated a process, it keeps the
CPU until it terminates. This type of scheduling is suitable for batch
scheduling.

## First Come First Serve
Scheduler chooses the process at the head of the queue of ready processes.

Average waiting times are often quite long

Causes a **convoy effect**: all processes need to wait for a big process to
release the CPU.

![fcfs](./pictures/fcfs.png)

In this example:
* B waits 2 cycles
* C waits 6 cycles
* In total, 8 cycles of waiting
* On average, 2.66 cycles

## Shortest Job First
Instead of choosing the first process in the queue, scheduler chooses the
shortest job first.

Reduces the waiting times dramatically, compared to FCFS.

However, we need to know processing times in advance, which can be difficult.
Long processes can be starved, if there are always new shorter jobs joining
the queue.

![sjf](./pictures/sjf.png)

In this example:
* B waits 3 cycles
* C waits 1 cycle
* In total, 4 cycles of waiting
* On average, 1.33 cycles
