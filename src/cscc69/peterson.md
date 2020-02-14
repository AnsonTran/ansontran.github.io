# Peterson's Solution
Assumptions:
* No special hardware instructions, no restriction on # of processors
* Basic machine language instructions (LOAD STORE, etc.) are atomic

## First Attempt
Share an int variable *turn*(0 or 1). If turn=i, thread i is allowed into its CS
```c
stuff(id_t id) {
	...
	while (turn != id); /* entry section */
	/* critical section, access protected resource */
	turn = 1-id; /* exit section */
	...
}
```
This achieves mutual exclusion, but not progress.
1. thread 1 enters, sets turn to 0
2. thread 1 wants to enter again, but thread 0 doesnt
3. thread 1 cannot enter

## Second Attempt
Have a shared flag for each thread
```c
boolean flag[2] = {false, false}
```
Each thread updates its own flag, and reads other thread's flag. If flag[i] is
true, thread i is ready to enter its CS.
```c
stuff(id_t id) {
	...
	while (flag[1-id]); /* entry section */
	flag[id] = true; /* indicate entering CS */
	/* critical section, access protected resource */
	flag[id] = false; /* exit section */
	...
}
```
However, if both flags reach the while loop at the same time (context switch),
they both set flag to true and enter critical section.

## Third Attempt (Peterson's Solution)
Peterson's solution combines both the first and second attempts.
```c
#define FALSE 0
#define TRUE 1
#define N 2 // number of processes

int turn; // whose turn is it
int interested[N]; // all values initially 0

void enter_region(int process) {
	int other; // number of other process

	other = 1-process; // the opposite of process
	interested[process] = TRUE; // show that you're interested
	turn=process; // set flag
	while (turn == process && interested[other] == TRUE); // null statement
}

void leave_region(int process) {
	interested[process] = FALSE; // indicate exit CS
} 
```
