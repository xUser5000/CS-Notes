# Explanation
- *Condition Variable*: explicit queue that threads can put themselves on when some state of execution is not as desired.

## API
- `wait()`: puts the calling thread to sleep.
	- Takes a #mutex as a parameter and assumes it's acquired.
	- Releases the lock and sleeps atomically.
	- Re-acquires the lock on return.
- `signal()`: wakes up a single sleeping thread waiting on the condition variable.

## Problems they solve
- Join problem (refer to [[Concurrency]]): when parent thread wants to check whether or not a child has completed before continuing.
- Producer/Consumer (AKA Bounded Buffer) Problem: two kinds of threads cooperate to put/consume data on a shared, bounded queue.
	- Used to implement multi-threaded web servers and Unix Pipes.

# Sources
- OSTEP Chapter 30 - "Condition Variables"