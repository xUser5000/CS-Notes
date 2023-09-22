# Explanation
- Scheduled by a runtime library or a virtual machine instead of natively by the operating system.
- Managed in #user_mode instead of #kernel_mode.
	- Runs on a single OS native thread.
- Works in environments that don't have native thread support.
- The name comes from the original thread library in Java 1.1.
	- They were later abandoned and replaced by native threads in Java 1.3.
- Must use asynchronous I/O (refer to [[Event-based Concurrency]]) since a traditional IO request would block the whole process.

# Sources
- [Wikipedia - Green Thread](https://en.wikipedia.org/wiki/Green_thread)