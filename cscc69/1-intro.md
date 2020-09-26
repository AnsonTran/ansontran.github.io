---
layout: post
title: "Introduction to Operating Systems"
---


# Introduction to Operating Systems

The operating system is a software layer between user applications and hardware

* Turns ugly hardware into abstractions (provides services)
* Serves as a resource manager
* Serves as a control program (protection)

Will focus on three core abstractions and resource management services:

* Processes and Threads
* Memory management
* File & I/O systems

![Operating System Overview](./pictures/os.png)

## Storage Hierarchy

* Processor registers, main memory, HDD/SDD, cloud services
* Registers are fast and close to the CPU, but small in size and expensive to
  have lots of
* Cloud services are a lot slower and far from CPU, but larger in size and
  cheaper
* Caches installed to hide performance differences, in large access-time gaps
  between two levels
	* DRAM store programs and data during program execution, but is volatile
	* We copy to cache memory close to the processor
	* But cache is limited, and cache management is necessary (coherency)

![Storage Hierarchy](./pictures/storage.png)

## Concurrency
Modern computers are multiprocessors, where CPU and device controllers execute
concurrently and compete for memory cycles.

Memory controller
 : Synchronizes access to shared memory

Hardware Interrupts
 : Used to signal the CPU of some event (disk I/O complete, network packet
   arrived, etc.)

Traps/Exceptions
 : Signal errors (division by zero) or requests OS service (system call)

Operating systems are event-driven programs.

## Processes
Processes are programs in execution, and contains all states of the program

* An address space
* Set of OS resources
* Set of general-purpose registers with current values
* Named by process ID (PID)

Independent proccesses
 : Cannot affect or be affected by other processes executing in the system (no
   data sharing)

Cooperating processes
 : Must communicate with each other and synchronize their actions
 : Shared memory (fork())
 : Message passing
 : send(P, msg) - send msg to process P
 : receive(Q, msg) - receive msg from process Q

### Process Data Structures
Processes are represented by the OS using a data structure called the Process
Control Block (PCB), which contains all the info about a process when it isnt
running

| State Variable      | Values                          |
| ------------------- | ------------------------------- |
| Process state       | ready, running, blocked         |
| Program counter     | address of the next instruction |
| CPU Registers       | must be saved at an interrupt   |
| CPU scheduling info | priority in the queue           |
| Page tables         | memory management info          |
| I/O status info     | list of open files              |

Processes must share the CPU, and the PCB is everything needed to restore a
process to its hardware configuration when the process was switched out of hardware

![**Linux PCB** - defined in /include/linux/sched.h](./pictures/task-struct.png)

### State Queues
The OS maintains a collection of queues which represent the state of all
processes in the system (typically one for each state)

1. OS dynamically allocates for a PCB and initializes a process, and places it
   in a ready queue
2. When changing state, PCB is unlinked from one queue and linked into another
3. Each PCB is requeued according to its state - ready, waiting, etc.
4. PCB is deallocated when the process terminates

### Context Switch
Switches the CPU to another process, saving the state of the old process and
loading the saved state of the next process in the ready queue.

Context switching is overhead, and a performance bottleneck, so threads are
used to avoid it

Processes are created using the fork() command

* Creates a new address space, with a **copy** of the entire contents of
  the address space of the parent
* Initializes kernel resources to point to the resources used by parent
* Resturns twice, child's pid to the parent and "0" to the child which we can use
  control flow of each process

### Forking Web Server
```c
while (1) {
	int sock = accept();
	if ((child_pid = fork()) == 0)
		# Handle client request
	} else {
		# Close socket
	}
}
```
![process-server](./pictures/process-server.png)

This concurrent web server forks a new process to handle every call.
Notice however, that all processes share:

* Code and data (address space)
* Privileges
* Resources (files, sockets, etc.)

The processes do not share:

* program counter
* stack pointer
* registers

Copying over the shared data for every process means there is some optimization
that is possible, which is where threads come in.

![thread](./pictures/thread.png)

## Threads
Separate the two into the concepts of *processes* and *threads*

* **Thread** defines a sequential execution stream within a process
* **Process** defines the address space and general process attributes (everything but threads of execution)

Threads are bound to a process, processes are containers for at least one thread

### Concurrent Thread Server
```c
web_server() {
	while (1) {
		int sock = accept()
		thread_fork(handle_request, sock);
	}
}
handle_request(int sock) {
	# Process request
	close(sock);
}
```

![pthread](./pictures/pthread.png)

Thread scheduler determines when a thread runs using queues. Unlike OS and processes, it is implemented
at user-level in a library.

* Run queue - Threads currently running (usually one)
* Ready queue - Threads ready to run
