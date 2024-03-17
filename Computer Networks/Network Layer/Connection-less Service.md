# Explanation
- If a connection-less service is offered by the network layer, packets are injected into the network individually and routed independently.
	- No advance setup is needed.
	- Packets in this context are called **datagrams**.
	- The network is called a **datagram network**.

## Mechanism
- The network layer breaks the message into packets and sends each of them to a router using some point-to-point protocol.
- Every router has an internal table telling it where to send packets for each of the possible destinations.
	- Each table entry is a pair consisting of a destination and the outgoing line to use for that destination.
	- Only directly-connected lines can be used, even if the ultimate destination is some other router.
- [[Routing Algorithms|Routing Algorithm]]: The algorithm that manages the tables and makes the routing decisions.
- The [[Internet Protocol (IP)]], which is the basis for the entire Internet, is the dominant example of a connectionless network service.
	- Each packet carries a destination IP address that routers use to individually forward each packet.
	- The addresses are 32 bits in IPv4 packets and 128 bits in IPv6 packets.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.1.3
