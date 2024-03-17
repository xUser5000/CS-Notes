# Explanation
- All TCP connections are full duplex and point-to-point.
	- *Full duplex* means that traffic can go in both directions at the same time.
	- *Point-to-point* means that each connection has exactly two end points.
- TCP does not support multicasting or broadcasting.
- A TCP connection is a byte stream, not a message stream.
	- Message boundaries are not preserved end-to-end.
	- If a process does four 512-byte writes to a TCP stream, these data may be delivered to the receiving process as four 512-byte chunks, two 1024-byte chunks, one 2048-byte chunk or some other way.
	- There is no way for the receiver to detect the unit(s) in which the data were written, no matter how hard it tries.
- When an application passes data to TCP, TCP may send it immediately or buffer it (in order to collect a larger amount to send at once).

## Sockets
- TCP service is obtained by both the sender and the receiver creating end points, called **sockets**.
	- A socket may be used for multiple connections at the same time.
- For TCP service to be obtained, a connection must be explicitly established between a socket on one machine and a socket on another machine.

## Ports
- Each socket has a socket number (address) consisting of the IP address of the host and a 16-bit number local to that host, called a **port**.
- Port numbers below 1024 are called well-known ports and are reserved for standard services that can usually only be started by privileged users (e.g., root in UNIX systems).
	- Some assigned ports: ![[Some well-known ports.png]]

## inetd (Internet daemon)
- Attaching daemons (e.g., like FTP or HTTP) at boot time would clutter up memory with daemons that are idle most of the time.
- *inetd*: A single daemon in UNIX that attaches itself to multiple ports and waits for the first incoming connection.
	- When a connection is established, inetd forks off a new process and executes the appropriate daemon in it.
- This way daemons other than inetd are only active when there is work for them to do.
- inetd learns which ports it is to use from a configuration file.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.5.2
