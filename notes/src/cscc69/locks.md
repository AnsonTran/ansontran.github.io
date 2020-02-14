# Locks

## Test-and-Set Lock (TSL) - Atomic Instruction
On single-core processors:
* Disable interrupts before entering critical section
* Prevents context switches

Uses a lock variable:
* Lock == 0 - nobody is using the lock
* Lock == 1 - lock is in use
* Must change lock's value when acquiring lock

```c
/* EXECUTES ATOMICALLY */
boolean test_and_set(boolean *lock) {
	boolean old = *lock; // record old value
	*lock = True; // set to some non-zero value
	return old; // return old value
}
```

*lock* is always true on exit
1. return true - locked already, nothing changed
2. return false - lock was available and is acquired

## Spinlock
Does busy waiting - thread continuously executes while loop in acquire()
```c
boolean lock;

void acquire(boolean *lock) {
	while(test_and_set(lock));
}

void release(boolean *lock) {
	*lock = false;
}
```

Using spinlocks, we can rewrite the banking example:
```c
int withdraw(acct, amt) {
	acquire(lock);
	...
	release(lock);
	return balance;
}

int deposit(acct, amt) {
	acquire(lock);
	...
	release(lock);
	return balance;
}
```

When using spinlocks, we need to release the lock when we are done with it.
Otherwise, other threads can't run and we cause deadlock.
