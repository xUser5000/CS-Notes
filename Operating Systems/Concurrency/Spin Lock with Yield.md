# Explanation
- Instead of spinning (as in [[Spin Lock]]) when trying to acquire a lock, we can use the `yield()` system call to give up the CPU entirely.
- `yield()` moves the caller thread from `running` state to `ready`.

## Mechanism
![[Code for Spin Lock with yield.png]]

## Advantages
- Extends [[Spin Lock#Advantages]]
- Faster than [[Spin Lock]] since it does not spin.

## Disadvantages
- Extends [[Spin Lock#Disadvantages]]
- The cost of a context switch can be quite substantial.

# Sources
- Extends [[Lock#Sources]]