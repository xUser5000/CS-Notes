# Explanation
- *Lock*: synchronization primitive that enforces #mutual_exclusion on a resource. [2]
	- ensures that any such critical section executes as if it were a single #atomic_instruction.
	- In POSIX terms, named #mutex.

## Mechanism
- Can be in two states: #locked / #unlocked.
- Only one thread can hold the lock at any given time.

# API
- `lock()`: acquires the lock
- `unlock`: releases the lock

# Implementations
- [[Controlling Interrupts]]
- Loads & Stores: does not provide #mutual_exclusion.
- [[Spin Lock]]
- [[Spin Lock with Yield]]
- [[Locks with Queues & Wakeup]]
- Linux-based Futex Locks
	- Optimizes for the common case: when there is no contention for the lock, very little work is done.

# Sources
1. OSTEP Chapter 28 - "Locks"
2. [Wikipedia - Locks](https://en.wikipedia.org/wiki/Lock_(computer_science))