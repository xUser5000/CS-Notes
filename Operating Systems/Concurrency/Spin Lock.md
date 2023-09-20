# Explanation
- A simple working lock that assumes hardware support via *test-and-set* (AKA atomic exchange) instruction.
	- ![[Code for test-and-set.png]]
- Requires a preemptive scheduler via #timer_interrupt to work.

## Mechanism
![[Code for Spin Lock using test-and-set.png]]

## Advantages
- Provides #mutual_exclusion.
- 

## Disadvantages
- Does not provide any fairness guarantees.
	- A thread spinning may spin forever and starve.
- Inefficient in terms of performance. (due to spinning)

# Sources
- Extends [[Lock#Sources]]