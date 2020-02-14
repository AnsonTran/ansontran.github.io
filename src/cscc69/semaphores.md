# Semaphores
Abstract data types that provide synchronization
* An integer variable, accessed only through 2 atomic operations
	* **wait** (also P or decrement) - decrement the variable and block until
	  semaphore is free
	* **signal** (also V or increment) - increment the variable, unblock a waiting thread * A queue of waiting threads

```c
wait(semaphore *s) {
	while (s->count == 0);
	s->count -=1;
}

signal(semaphore *s) {
	s->count += 1;
	... //unblock one waiter
}
```

Have to ensure that two threads cannot execute *wait* and *signal at the same
time

There are two types of semaphores:
1. Mutex (binary) semaphore
	* single access to a resource
	* guarantees mutual exclusion to a critical section
	* used for single process access to a single core CPU
2. Counting semaphore
	* resource with many units, allows certain kinds of unsynchronized concurrent
	  access (e.g., reading)
	* multiple threads can pass the semaphore
	* max number of threads determined by semaphore's intial count value

## Semaphores Banking Problem
```c
typedef struct account {
	double balance;
	semaphore S;
} account_t;

int withdraw(account_t *acct, amt) {
	double bal;
	wait(acct->S);
	bal = acct->balance;
	bal = bal - amt;
	acct->balance = bal;
	signal(acct->S);
	return bal;
}
```

## Semaphores Readers/Writers Problem
Use a semaphore. Think of it as a token held by either a group of readers or one
individual writer. Use three variables:
* Semaphore w_or_r - exclusive writing or reading
* Semaphore mutex - control access to readcount
* int readcount - number of threads reading object

Note that *readcount* is itself a shared resource and needs a mutex to sync

Its useful to think of reader as entering/leaving a classroom. First one has to
turn on the lights, and last to leave has to turn off the lights.

```c
int readcount = 0;
Semaphore mutex = 1;
Semaphore w_or_r = 1;

Writer {
	wait(w_or_r);
	Write;
	signal(w_or_r);
}

Reader {
	wait(mutex); // lock readcount
	readcount += 1;

	// is this the first reader?
	if (readcount == 1)
		wait(w_or_r); // synch w/ writers

	// unlock readcount
	signal(mutex);
	Read;

	wait(mutex); // lock readcount
	readcount -= 1;

	// last reader needs to signal
	if (readcount == 0)
		signal(w_or_r);
	signal(mutex);
}
```

## Semaphores Dining Philosophers Problem
When a philosopher tries to take forks, they record that they're hungry,
and tries to take both forks. If they can't, they block.

It is only when the philosopher next to them has finished eating, that they
wake their neighbours to test again if they're hungry.

An alternate solution is: when a philosopher is hungry, to continuously check
their left and right forks in a cycle.

```c
#define N 5
#define LEFT (i+N-1)%N
#define RIGHT (i+1)%N
#define THINKING 0
#define HUNGRY 1
#define EATING 2
typedef int semaphore;
int state[N];
semahore mutex = 1; // mutual exclusion to critical sections
semaphore s[N]; // one semaphore per philosopher

void philosopher(int i) {
	while (TRUE) {
		think();
		take_forks(i); // acquire two forks or block
		eat();
		put_forks(i); // put both forks back on the table
	}
}

void take_forks(int i) {
	down(&mutex); // enter critical section
	state[i] = HUNGRY;
	test(i); // try to acquire 2 forks
	up(&mutex); // exit critical section
	down(&s[i]); // block if forks not acquired
}

void put_forks(int i) {
	down(&mutex);
	state[i] = THINKING; // finished eating
	test(LEFT); // see if left neighbour can now eat
	test(RIGHT); // see if right neighbour can now eat
	up(&mutex);
}

void test(i) {
	if (state[i] == HUNGRY && state[LEFT] != EATING && state[RIGHT] != EATING) {
		state[i] = EATING;
		up(&s[i]);
	}
}
```
