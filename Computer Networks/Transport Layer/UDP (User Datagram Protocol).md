# Explanation
- *UDP*: a connection-less protocol that does nothing beyond sending packets between applications, letting applications build their own protocols on top.
	- It provides a way for applications to send encapsulated IP datagrams without having to establish a connection.
- The main value of UDP over just using raw IP is the addition of the source and destination ports.
	- With them, it delivers the embedded segment to the correct application.
- UDP typically runs in the operating system and protocols that use UDP typically run in user space.

## Header Structure
![[UDP Header.png]]
- UDP transmits segments consisting of an 8-byte header followed by the payload.
- The two ports serve to identify the endpoints within the source and destination machines.
- When a UDP packet arrives, its payload is handed to the process attached to the destination port.
	- This attachment occurs when the `BIND` primitive or something similar is used.
- The source port is primarily needed when a reply must be sent back to the source.
- The UDP length field includes the 8-byte header and the data.
	- The minimum length is 8 bytes, to cover the header.
	- The maximum length is 65,515 bytes, which is lower than the largest number that will fit in 16 bits because of the size limit on IP packets.
- The optional checksum field includes the header, the data, and a conceptual IP pseudo-header for extra reliability.

## Advantages
- For applications that need to have precise control over the packet flow, error control, or timing, UDP provides just what the doctor ordered.
- Requires fewer messages than [[TCP]].
- Does not require initial setup (i.e., connection).
- Does not require a release step.

## Disadvantages
- Does not do flow control, congestion control, or retransmission upon receipt of a bad segment.
	- All of that is up to the user processes.

## Applications
- [[DNS]]:
	- A program that needs to look up the IP address of some host name can send a UDP packet containing the host name to a DNS server.
	- The server replies with a UDP packet containing the host’s IP address.
	- This whole process does not require any setup in advance and no releases is needed afterwards, just two messages over the network.
- RPC (Remote Procedure Calls)
- Real-time streaming service

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.4
