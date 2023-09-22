# Explanation
- Basic Idea: "Wait for something (i.e, event) to occur; when it does, you check what type of event it is and do the small amount of work it requires (which may include issuing I/O requests, or scheduling other events for future handling, etc.)".
- #synchronous: interfaces do all their work before returning to the caller.
- #asynchronous: interfaces begin some work but return immediately, thus letting whatever work that needs to be done get done in the background.
- No call that blocks the execution of the caller can ever be made.
	- Failing to obey this design tip will result in a blocked #event_loop.
## Mechanism
- Illustration of the #event_loop
```C
while (1) {
	events = getEvents();
	for (e in events)
		processEvent(e);
}
```
- The code that processes events is known as the #event_handler.
- `getEvents()` can be implemented using the `select()` system call in Linux.
- #asynchronous IO must be used to enable applications to issue an IO request and return control immediately to the caller, before the IO has completed.
	- Event-based concurrency model cannot be implemented in systems without #asynchronous IO.

## Advantages
- Solves problems in thread-based [[Concurrency]]:
	- Managing multi-threaded programs correctly is difficult
		- e.g, [[Concurrency#Bugs]]
	- The developer has little to no control over what is scheduled at a given moment in time.
- No need to acquire or release locks.
	- The event loop cannot be interrupted by another thread since it's the only thread running.
- Doesn't suffer from context switches. [2 - 5:31]
- Uses less memory, in contrast to [[Thread]]s. [2 - 5:31]

## Disadvantages
- In multi-processor systems, some of the simplicity disappears.
	- Not possible to utilize more than one CPU without using locks.
- Does not integrate well with certain kinds of systems activity such as [[Paging]].

# Sources
1. OSTEP Chapter 33 - "Event-based Concurrency (Advanced)"
2. [JSConf - Ryan Dahl: Node JS](https://www.youtube.com/watch?v=EeYvFl7li9E)