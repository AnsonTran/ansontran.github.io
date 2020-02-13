# Synchronization
Processes might want to communite with each other to do certain tasks. A
process is: * **independent** if it cannot affect/be affected by other processes executing in the system. No data sharing * **cooperating** if not independent. Needs data sharing

Cooperating processes can exchange information using either
* Shared memory (e.g. fork())
* Message passing - Send(P,msg) and Receive(Q,msg)

High-level Abstractions of the critical section:
* **Locks** - Very primitive, minimal semantics
* **Semaphores** - Basic, easy to understand, hard to program with
* **Monitors** - High-level, ideally language support (Java)
* **Messages** - Model for communication and sync. Direct application to
  distributed systems.

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
This piece of code needs to be synchronized so that only one thread can enter
its critical section at a time. Other processes running concurrently must wait.

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
2. **Progress** - If no thread in CS and a thread wants to enter CS, it should
   be able to enter in definite time.
3. **Bounded waiting** - If some thread wait on CS, there should be a limit on
   the number of times other threads can enter CS before this thread is granted
   access. (no starvation)
4. **Performance** - Overhead of enter/exit is small with respect to work being
   done within it

## Producer and Consumer Problem
Processes share a bounded buffer. The producer puts info into the buffer, and
consumers take info out.

Producer sleeps when buffer is full, and wakes up consumer when producing an item

Consumer sleeps when buffer is empty, and wakes up producer when consuming an
item

```c
#define N 100
int count = 0;

void producer(void) {
	int item;
	while (TRUE) {
		item = produce_item();
		if (count == N) sleep();
		insert_item(item);
		count += 1;
		if (count == 1) wakeup(consumer);
	}
}

void consumer(void) { 
	int item; 
	while (TRUE) {
		if (count == 0) sleep();
		item = remove_item();
		count -= 1;
		if (count == N-1) wakeup(producer);
		consume_item(item);
	}
}
```

## Readers/Writers Problem
An object is shared among several threads. Some threads only read the object,
while other threads only write to it.

We want to allow multiple concurrent **readers**, but only one **writer**

![readers-writer](./pictures/readers-writer.png)

## Dining Philosophers
In this problem, there are N philosophers sitting around a table. There is N
bowls of spaghetti and N forks. However, each philosopher needs two forks to eat.
Philosophers can indicate to others that they want to eat by entering the HUNGRY
state. When they are eating, they are in the EATING state. When not eating and
not hungry, the philosopher is THINKING.

![dining-philosophers](./pictures/dining-philosophers.png)
