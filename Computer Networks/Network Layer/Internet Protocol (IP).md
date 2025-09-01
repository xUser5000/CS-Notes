# Explanation
- The glue that holds the whole Internet together is the network layer protocol, IP (Internet Protocol).

## Communication in the Internet
1. The [[Transport Layer|transport layer]] takes data streams and breaks them up so that they may be sent as IP packets.
	- In theory, packets can be up to 64 KB each, but in practice they are usually not more than 1500 bytes (so they fit in one Ethernet frame).
2. IP routers forward each packet through the Internet, along a path from one router to the next, until the destination is reached.
3. At the destination, the [[Network Layer|network layer]] hands the data to the transport layer, which gives it to the receiving [[Process|process]].
4. When all the pieces finally get to the destination machine, they are reassembled by the network layer into the original [[Connection-less Service#^eae0cb|datagram]].
5. This datagram is then handed to the transport layer.

# Topics
- [[IP Version 4 Protocol]]
- [[IP Addresses]]
- [[Internet Control Protocols]]
	- [[Internet Control Message Protocol (IMCP)]]
	- ARP
	- [[Dynamic Host Configuration Protocol (DHCP)]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.6
