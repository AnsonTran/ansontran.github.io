# Synchronization (No hardware)
Processes might want to communite with each other to do certain tasks. A
process is:
* **independent** if it cannot affect/be affected by other processes executing
  in the system. No data sharing
* **cooperating** if not independent. Needs data sharing

Cooperating processes can exchange information using either
* Shared memory (e.g. fork())
* Message passing - Send(P,msg) and Receive(Q,msg)

## Banking Example
Suppose we write a function to handle withdrawals and deposits to a bank account.
A bank's central server would create separate threads for each action.

```c
int balance = 1000;

int withdraw(acct, amt) {
	balance = get_balance(acct);
	balance = balance - amt;
	put_balance(acct, balance);
	return balance;
}

int deposit(acct, amt) {
	balance = get_balance(acct);
	balance = balance + amt;
	put_balance(acct, balance);
	return balance;
}
```

Suppose you withdraw $100 and S.O. deposits $100. What is the account balance?
The problem is, processes can be interleaved, due to context switching.

```c
// Withdrawing code
balance = get_balance(acct);
balance = balance - amt;

// Context switch, deposit code starts executing
balance = get_balance(acct);
balance = balance + amt;
put_balance(acct, balance);

// Withdrawing code continues
put_balance(acct, balance);
```

The problem is that concurrent threads manipulated a shared resource without
synchronization. This causes a **race condition**, where outcome depends on the
order in which accesses take place.

## Critical Section
We identify a section of the code where shared resources are accessed, called the
**critical section**.

```c
int balance = 1000;

int withdraw(acct, amt) {
	// Enter critical section
	balance = get_balance(acct);
	balance = balance - amt;
	put_balance(acct, balance);
	// Exit critical section
	return balance;
}

int deposit(acct, amt) {
	// Enter critical section
	balance = get_balance(acct);
	balance = balance + amt;
	put_balance(acct, balance);
	// Exit critical section
	return balance;
}
```
This piece of code needs to be synchronized so that only one thread can enter its
critical section at a time. Other processes running concurrently must wait.

For program data:
* Local variables are not shared - *private*
	* Each thread has its own stack
	* Local variables allocated on this private stack
* Global variables and static objects - *shared*
	* Stored in static data segment, accessible by any thread
* Dynamic objects and other heap objects - *shared*
	* Allocated from heap with malloc/free or new/delete

We want to design a protocol that satisfies the following properties:
1. **Mutual Exclusion** - If a thread is in CS, no other thread is
2. **Progress** - If no thread in CS and a thread wants to enter CS, it should be
   able to enter in definite time.
3. **Bounded waiting** - If some thread wait on CS, there should be a limit on
   the number of times other threads can enter CS before this thread is granted
   access. (no starvation)
4. **Performance** - Overhead of enter/exit is small with respect to work being
   done within it

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
