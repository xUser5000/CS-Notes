# Explanation

## Definitions
- #parallelism: when two or more threads are executing simultaneously. [2]
- #concurrency: when two or more threads are making progress.
	- A more generalized form of parallelism that can include #time_sharing as a form of virtual parallelism. [2]
- [[Thread#Definition]]
- #multithreaded_program: a program that allows access to two or more [[Thread]]s. [4]
	- Each thread has its own #call_stack. Illustration: ![[Single-Threaded And Multi-Threaded Address Spaces.png]]
- #race_condition (AKA #data_race): When multiple threads enter the #critical_section at the same time and update a shared data structure. [1]
	- Results depend on the timing execution of the code.
- #critical_section: a piece of code that accesses a shared resource and must not be concurrently executed by more than on thread. [1]
- #mutual_exclusion: a property that guarantees if one thread is executing within the #critical_section, the others will be prevented from doing so. [1] 
- #atomic_instruction: an instruction that either executes entirely or does not execute at all. [1]
	- It is never partially executed.
	- "all or nothing"

## Primitives
- [[Lock]]
- [[Condition Variable]]
- [[Semaphore]]

## Bugs
- Non-deadlock:
	- Atomicity Violation: desired serviceability among multiple memory accesses is violated.
	- Order Violation: desired order between two memory accesses is flipped.
- [[Deadlock]]

## Other Concurrency Models
- [[Event-based Concurrency]]

# Sources
1. OSTEP Chapter 26 - "Concurrency: An Introduction"
2. [Stack Overflow - What is the difference between concurrency and parallelism?](https://stackoverflow.com/a/1050257)
3. [Wikipedia - Thread (computing)](https://en.wikipedia.org/wiki/Thread_(computing))
4. [Oracle - Defining Multithreading terms](https://docs.oracle.com/cd/E19455-01/806-5257/6je9h032b/index.html)
5. OSTEP Chapter 32 - "Common Concurrency Problems"