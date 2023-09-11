# Explanation
- It's a bug that occurs when a program writes more data to a buffer located on the #call_stack than what is actually allocated for that buffer.
- It can be a potential security vulnerability if an attacker tries to deliberately trigger it as part of an attack known as *stack smashing*.
	1. The attacker injects malicious code into the buffer.
	2. Overrides the return address of the current function so that it points to the start of the malicious code.
	3. When the function jumps to the return address, it starts executing the injected malicious code.
- Buffer overflow bugs can allow remote machines to execute arbitrary code on victim machines. 
- How to defend against buffer overflows?
	1. Programmers should avoid code vulnerabilities.
		- e.g. use library routines that limit string lengths
	2. Address Space Layout Randomization (ASLR)
		- Each time the program starts executing, the system allocates a random amount of unused space on the #call_stack (i.e padding).
		- Shifts all #call_stack addresses by a constant offset which makes it difficult for attackers to predict the beginning of inserted code.
	3. Non-executable code segments
		- By default, the system marks the #call_stack segment as non-executable so that an attacker cannot execute an injected code.
	4. #call_stack canaries
		- The system places a special value (the "canary") just after the buffer and checks for corruption before the function exits.
		- `-fstack-protector` in GCC (now enabled by default).

# Sources
- [CMU Introduction to Computer Systems - Lecture 9](https://scs.hosted.panopto.com/Panopto/Pages/Sessions/List.aspx#folderID=%22b96d90ae-9871-4fae-91e2-b1627b43e25e%22)
- [Wikipedia - Stack Buffer Overflow](https://en.wikipedia.org/wiki/Stack_buffer_overflow)