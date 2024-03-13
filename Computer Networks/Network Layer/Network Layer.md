# Explanation
- The goal of the network layer is to deliver packets from the source machine all the way to the destination machine.
	- Getting to the destination may require making many hops at intermediate routers along the way.
	- This is different from the data link layer, whose goal is to just move frames from one end of a wire to the other.

## Mechanism
- The mechanism used by the network layer to deliver packets is called **Store-and-Forward Packet Switching**:
	1. A host with a packet to send transmits it to the nearest router, either on its own LAN or over a point-to-point link to the ISP.
	2. The packet is stored there until it has fully arrived and the link has finished its processing by verifying the checksum.
	3. Then it is forwarded to the next router along the path until it reaches the destination host, where it is delivered.
- Illustration: ![[Environment of the network layer protocols.png]]

## Services
- The services provided to the transport layer need to be carefully designed with the following goals in mid:
	- The services should be independent of the router technology.
	- The transport layer should be shielded from the number, type, and topology of the routers present.
	- The network addresses made available to the transport layer should use a uniform numbering plan, even across LANs and WANs.
- There are two types of services that can be provided by the network layer:
	- [[Connection-less Service]]
	- [[Connection-oriented Service]]

# Topics
- [[Routing Algorithms]]
- [[Congestion Control]]

# FAQ
- What's the difference between connection-less services and connection-oriented ones?
	- Answer: ![[Comparison of datagram and virtual-circuit networks.png]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Introduction of Chapter 5
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.1.1
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.1.2
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.1.5
