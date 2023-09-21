# Explanation

## Definition
- Smallest sequence of programmed instructions that can be managed independently by a scheduler.
- Sequence of instructions executed within the context of a [[Process]].
- Single point of execution within a program.

## Characteristics
- Threads within the same process share the same address space and access the same data.
- On #context_switch, the address space remains the same.

## Benefits
- Speed up computation via #parallelism.
- Avoid blocking program progress due to slow IO.
	- While on thread waits, the scheduler can switch to other threads.
- Easily share data since threads live in the same address space, unlike processes.

## Issues
- Uncontrolled Scheduling: hard to tell which thread will run next.
	- Can be solved using [[Condition Variable]]s or [[Semaphore]].
- Vulnerable to #race_condition s.
	- Can be solved using [[Lock]]s

# Sources
- Extends [[Concurrency#Sources]]