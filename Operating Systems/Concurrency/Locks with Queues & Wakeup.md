# Explanation
- Assumes a hardware support via:
	- `park()` to put the calling thread to sleep.
	- `unpark(thread_id)` to wake a particular thread designated by `threadid`.
- Uses a queue to keep track of the waiting thread in order.

## Mechanism
- Illustration: ![[Lock with Queues & Wakeup.png]]
- Corner case: the scheduler switches from a thread that is about to `park()` after releasing the guard. When that thread wakes again, it will call `park()` and sleep forever. (AKA wakeup/waiting race).
	- Can be solved via a call to `setpark()` as follows: ![[setpark().png]]

## Advantages
- Extends [[Spin Lock with Yield#Advantages]]
- Faster since there is no overhead of the context switch.
- Solves the starvation problem.

## Disadvantages


# Sources
- Extends [[Lock#Sources]]