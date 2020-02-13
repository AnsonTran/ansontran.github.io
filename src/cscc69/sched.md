# Process Scheduling
On a CPU, only one process can run at a time. We will need a scheduler that
decides which processes to run. We want a CPU scheduling algorithm that
achieves:
* **Context switching** - Gives illusion of concurrency (single core)
* **Fairness** - Each process receives fair share of CPU
* **Avoid starvation** - Process should get CPU in definite time 
* **Policy enforcement** - Usage policies should be met
* **Balance** - All parts of the system should be busy
* **CPU utilization** - Keep the CPU busy all the time

Preemptive scheduling is suitable for real-time systems. The CPU can be taken
from a running process and allocated to another

On batch systems, we batch processes together and run them in batches. On these
systems, we want:
* **Throughput** - Maximize jobs completed per hour
* **Turnaround time** - Minimize time between submission and completion
* **CPU utilization** - Keep the CPU busy all the time

Non-preemptive scheduling is suitable for batch systems. Once the CPU is
allocated a process, it keeps the CPU until it terminates.

## Process States
OS manages processes by keeping track of their state.

During execution, a process in the running state:
* May be waiting for an I/O device or some condition.  
  They would then enter the blocked state.
* May time out.  
  OS takes back CPU and takes the next process in the ready queue

A process in the blocked state:
* May become ready to execute (I/O task is complete)  
  Process enters the ready queue

A process in the ready state:
* May be selected by the scheduler
  CPU takes the process, restores its state and starts to execute it

![process-cycle](./pictures/process-cycle.png)

## Dispatch/Context Switch
During a context switch, CPU switches to another process.
1. Save currently running process state (unless process is exiting)
2. Select next process from ready queue
3. Restore state of next process
	* registers
	* switch to user mode
	* set PC to next instruction in the process

## CPU and I/O Bursts
During I/O, the CPU is not needed. This is an opportunity for the CPU to execute
a process that does.

Processes alternate between CPU bursts and I/O bursts.
* Last CPU burst ends with a call to terminate the process
* CPU-bound - very long CPU bursts, infrequent I/O bursts
* I/O bound - short CPU bursts, frequent (long) I/O bursts

![bursts](./pictures/bursts.png) 
