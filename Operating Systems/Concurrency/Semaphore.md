# Explanation
- *Semaphore*: object with an integer value that can be manipulated with two atomic routines:
	- `wait()`:
		- Decrement the value of the semaphore by one
		- Wait if the value of the semaphore is negative
	- `post()`:
		- increment the value of the semaphore by one
		- If there are one or more threads waiting, wake one
- The initial value of the semaphore determines its behavior.
	- Indicates the number of resources we are willing to give away immediately after initialization.

## Implementation
![[Implementing Semaphores with Locks & CVs.png]]

## Use Cases
- Semaphores can be used as
	- [[Lock]]s by initializing them to one, or
	- [[Condition Variable]] to order events in a concurrent program.

## Problems they solve
- Producer/Consumer (Bounded Buffer) Problem.
- Implementing reader/writer locks.
- The Dinning Philosophers Problem 

# Sources
- OSTEP Chapter 31 - "Semaphores"