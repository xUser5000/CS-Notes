# Explanation
- *TCP*: a connection-oriented transport protocol that was specifically designed to provide a reliable end-to-end byte stream over an unreliable internetwork.
- Each machine supporting TCP has a TCP transport entity, either a library procedure, a user process, or most commonly part of the [[Operating Systems|OS]] kernel.
- A TCP entity:
	- accepts user data streams from local processes,
	- breaks them up into pieces not exceeding 64 KB,
	- and sends each piece as a separate IP datagram.
- When datagrams containing TCP data arrive at a machine, they are given to the TCP entity, which reconstructs the original byte streams.

## Topics
- [[TCP Service Model]]
- [[TCP Header]]
- [[TCP Connection Establishment]]
- [[TCP Connection Release]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.5
