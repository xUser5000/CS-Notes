# Explanation
- *Deadlock*: situation in which no thread in a group of threads can proceed because each waits for another thread, including itself, to release a lock. [2]
- Arises in many concurrent systems with complex locking protocols. [1]

## Conditions [1]
- #mutual_exclusion
- Hold-and-wait: Threads hold resources allocated to them while waiting for additional resources.
- No preemption: Resources cannot be forcibly removed from threads that are holding them.
- Circular Wait: There exists a circular chain of threads such that each thread holds one or more resources that are being requested by the next thread in the chain.

## Prevention [1]
- Write locking code such that it never induces a circular wait.
	- e.g, enforcing a total ordering on resource allocations.
- Acquire locks all at once (atomically).
	- Disadvantage: reduces concurrency.
- Call `trylock()` and if not successful, release all the held locks.
- Write lock-free code.

## Recovery [1]
- We can allow deadlocks to rarely occur, and then take some action once such deadlock has been detected.
- A deadlock detection thread runs periodically, building a resource allocation graph and checking it for cycles.
	- In the event of a deadlock, the system has to be restarted, or a more fine-grained action can be taken.

# Sources
1. OSTEP Chapter 32 - "Common Concurrency Problems"
2. [Wikipedia - Deadlock](https://en.wikipedia.org/wiki/Deadlock)