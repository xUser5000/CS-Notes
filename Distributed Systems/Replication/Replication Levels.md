# Explanation
- **Application State**: e.g., database tables. ^518c94
	- Can be efficient; primary only sends high-level operations to backup.
	- The downside is application code (server) must understand fault tolerance, to e.g. forward operations stream.
	- [[The Google File System|GFS]] works this way.
- **Machine level**: e.g. registers and RAM content
	- Might allow us to replicate any existing server without modifications.
	- Downsides:
		- requires forwarding of machine events (interrupts, DMA, &c), and
		- requires "machine" modifications to send/receive event stream.
		- [[Fault-tolerant Virtual Machines|VM-FT]] works this way.

# Sources
- [MIT 6.824 - Lecture 4](https://www.youtube.com/watch?v=M_teob23ZzY)
