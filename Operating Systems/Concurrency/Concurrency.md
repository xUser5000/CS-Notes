# Explanation
- #parallelism: when two or more threads are executing simultaneously. [2]
- #concurrency: when two or more threads are making progress. A more generalized form of parallelism that can include time-slicing as a form of virtual parallelism. [2]
- #thread:
	- Definition:
		- Smallest sequence of programmed instructions that can be managed independently by a scheduler. [3]
		- Sequence of instructions executed within the context of a [[Process]]. [4]
		- Single point of execution within a program. [1]
	- Characteristics:
		- Threads within the same process share the same address space and access the same the data.
		- On #context_switch, the address space remains the same.
	- Benefits: [1]
		- Speed up computation via #parallelism.
		- Avoid blocking program progress due to slow IO.
			- While on thread waits, the scheduler can switch to other threads.
		- Easily share data since threads live in the same address space, unlike processes.
	- Drawbacks: [1]
		- Uncontrolled Scheduling: hard to tell which thread will run next.
		- Vulnerable to #race_condition s.
- #multithreaded_program: a program that allows access to two or more threads. [4]
	- Each thread has its own #call_stack. Illustration: ![[Single-Threaded And Multi-Threaded Address Spaces.png]]
- #race_condition (AKA #data_race): When multiple threads enter the #critical_section at the same time and update a shared data structure. [1]
	- Results depend on the timing execution of the code.
- #critical_section: a piece of code that accesses a shared resource and must not be concurrently executed by more than on thread. [1]
- #mutual_exclusion: a property that guarantees if one thread is executing the within the #critical_section, the others will be prevented from doing so. [1] 
- #atomic_instruction: an instruction that either executes entirely or does not execute at all. [1]
	- It is never partially executed.
	- "all or nothing"

# Sources
1. OSTEP Chapter 26 - "Concurrency: An Introduction"
2. [Stack Overflow - What is the difference between concurrency and parallelism?](https://stackoverflow.com/a/1050257)
3. [Wikipedia - Thread (computing)](https://en.wikipedia.org/wiki/Thread_(computing))
4. [Oracle - Defining Multithreading terms](https://docs.oracle.com/cd/E19455-01/806-5257/6je9h032b/index.html)