# Deadlock

Many problems involve the production and consumption of some resource by
processes. These can be:
* Reusable - Used by one process at a time, released and used by another process
	* printers, memory, processors, files
	* locks, semaphores, monitors
* Consumable - dynamically created and destroyed. Allocated only once
	* interrupts, signals, messages

Recall this attempted solution to the critical section problem.
```c
My_work(id_t id) { /* id can be 0 or 1 */
	...
	flag[id] = true;
	while (flag[1-id]);
	/* critical section */
	flag[id] = false;
	...
}
```
If both processes set their flag to true, and entered the while loop, they will
both be waiting for the other to enter the critical section. This problem of
processes being **permanently** blocked is called deadlock, either through
competing for resoures, or communicating with each other. 

This problem can occur in real life as well! One notable example is an early
20th century Kansas law:

> When two trains approach each other at a crossing, both shall come to a full
> stop and neither shall start again until the other has gone

Locking example:
1. Processes P and Q need resources locks A and B to proceed
2. P gets lock A
3. Q gets lock B
4. Both are waiting for the other to release the other lock

Philosophers example (refer to dining philosophers problem for details):
1. All philosophers get hungry and grab right fork
2. All philosophers try to grab left fork, but it is taken
