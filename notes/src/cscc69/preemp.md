# Preemptive Scheduling
As we saw, both of the non-preemptive scheduling algorithms had drawbacks.
Preemptive algorithms dont always run long processes until completion, but allow
other processes to take the CPU before it finishes.

## Round Robin
In round-robin, we cycle through a circular ready queue.

Each process is allowed to run for time quantum *q*. If it takes longer, it is
preempted and put back on queue.
* \\(q \rightarrow \infty\\), RR \\(\rightarrow \\) FCFS
* \\(q \rightarrow 0\\), RR \\(\rightarrow \\) processor sharing (PS)

We want *q* to be large with respect to the context switch time

## Priority Scheduling
Associate a priority *p* with each process. Highest priority job is selected from
ready queue.

Enforcing this policy is tricky:
* Low priority task may never get to run (starvation)
* Low priority task may prevent high priority task from making progress by
  holding a resource (priority inversion)

![pathfinder-bug](./pictures/pathfinder-bug.png)

In this Mars Rover Pathfinder bug, there is a shared "information bus" resource.

1. Low priority "data gathering" is executing and acquires the lock to the bus
2. High priority "bus management" takes the CPU from the low priority task
3. Task is blocked from information bus by data gathering process

## Multi-Level Queue Scheduling
Multiple queues and two scheduling policies:
* First decides which queue to serve (queue priorities)
* Second decides which job within queue to choose (FCFS or RR)

Highest priority queue gets 1 quantum, second gets 2,..., etc.

If highest doesn't finish during quantum, bump down to second highest queue.

Shortest (high priority) jobs get out first

![multi-level-queue](./pictures/multi-level-queue.png)

## Feedback Scheduling
When scheduling, we want to achieve:
* priority to shorter jobs
* priority to IO bound jobs
* priority to interactive jobs

However, we don't know beforehand what type of jobs we receive. So, we can adjust
our criteria based on past history. Using feedback scheduling, combined with MLQ,
we can:
* prefer processes that do not use full quantum
* change priority of processes based on age
* change priority of processes based on CPU consumed so far
* boost priority following a user-input event
* move processes between queues

## Linux 2.6 CPU Scheduling
Linux uses a combination of multilevel queues (with priorites and RR) and
feedback scheduling.

Distinguishes between 3 classes:
* realtime FIFO processes
* realtime RR processes
* timesharing processes (we will focus on these)

The CPU always runs the task in active array with highest priority for a certain
**timeslice**. Processes with the same priority do RR between them.

![linux-active-sched](./pictures/linux-active-sched.png)

After **granularity** time units, array is expired. Next array becomes active.

![linux-expired-sched](./pictures/linux-expired-sched.png)

Priorities can be assigned to a static priority *nice* through the **nice(inc)**
system call. The calling thread adds **inc** to their nice value within the range
[-20,19].

On top of static **nice** priority, linux adds a bonus/penalty on top of it.
Interactive and IO bound processes are favoured. They can be identified by
looking at their **sleep_avg**. A high sleep_avg indicates an Interactive/IO
process.

```c
priority = static_priority + bonus
```

Timeslice depends on the static *nice* priority, usually between 5ms and 800ms.

Granularity depends on the dynamic priority
