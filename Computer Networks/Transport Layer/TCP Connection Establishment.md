# Explanation
- Extends [[Connection Establishment]].
- Connections are established in TCP by means of the [[Connection Establishment#Three-way Handshake|Three-way Handshake]].

## Mechanism
1. To establish a connection, one side, say, the server, passively waits for an incoming connection by executing the `LISTEN` and `ACCEPT` primitives in that order, either specifying a specific source or nobody in particular.
2. The other side, say, the client, executes a `CONNECT` primitive, specifying the IP address and port to which it wants to connect, the maximum TCP segment size it is willing to accept, and optionally some user data.
	- The `CONNECT` primitive sends a TCP segment with the SYN bit on and ACK bit off and waits for a response.
3. When the connection segment arrives at the destination, the TCP entity there checks to see if there is a process that has done a LISTEN on the port given in the Destination port field.
	1. If not, it sends a reply with the RST bit on to reject the connection.
	2. If some process is listening to the port, that process is given the incoming TCP segment and can either accept or reject the connection.
		-  If the connection request is accepted, an acknowledgement segment is sent back.

## Illustration
![[TCP Connection Establishment.png]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.5.4
