# Explanation

## Overview
- Every TCP **segment** begins with a fixed-format, 20-byte header.
- The fixed header may be followed by header options.
- After the options, if any, up to 65,535 − 20 − 20 = 65,495 data bytes may follow, where the first 20 refer to the IP header and the second to the TCP header.
- Segments without any data are legal and are commonly used for acknowledgements and control messages.

## Illustration
![[The TCP header.png]]

## Details
- The **Source port** and **Destination port** fields identify the local end points of the connection.
	- A TCP port plus its host’s IP address forms a 48-bit unique end point.
	- The source and destination endpoints together identify the connection.
- The TCP **header length** tells how many 32-bit words are contained in the TCP header.
	- Needed because the Options field is of variable length, so the header is, too.
- Next comes a 4-bit field that is not used.
	- The fact that these bits have remained unused for 30 years (as only 2 of the original reserved 6 bits have been reclaimed) is testimony to how well thought out TCP is.
	- Lesser protocols would have needed these bits to fix bugs in the original design.
- Now come eight 1-bit flags:
	- **CWR** and **ECE** are used to signal congestion when ECN ([[Traffic Throttling#Explicit Congestion Notification|explicit congestion notification]]) is used.
		- ECE is set to signal an ECN-Echo to a TCP sender to tell it to slow down when the TCP receiver gets a congestion indication from the network.
		- CWR is set to signal Congestion Window Reduced from the TCP sender to the TCP receiver so that it knows the sender has slowed down and can stop sending the ECN-Echo.
	- **URG** is set to 1 if the Urgent pointer is in use.
		- The Urgent pointer is used to indicate a byte offset from the current sequence number at which urgent data are to be found.
	- The **ACK** bit is set to 1 to indicate that the Acknowledgement number is valid.
		- This is the case for nearly all packets.
		- If ACK is 0, the segment does not contain an acknowledgement, so the Acknowledgement number field is ignored.
	- The **PSH** bit indicates PUSHed data.
		- The receiver is hereby kindly requested to deliver the data to the application upon arrival and not buffer it until a full buffer has been received (which it might otherwise do for efficiency).
	- The **RST** bit is used to abruptly reset a connection that has become confused due to a host crash or some other reason.
		- It is also used to reject an invalid segment or refuse an attempt to open a connection.
		- In general, if you get a segment with the RST bit on, you have a problem on your hands.
	- The **SYN** bit is used to establish connections.
		- The connection request has `SYN = 1 and ACK = 0` to indicate that the piggyback acknowledgement field is not in use.
		- The connection reply does bear an acknowledgement, however, so it has `SYN = 1 and ACK = 1`.
		- In essence, the SYN bit is used to denote both CONNECTION REQUEST and CONNECTION ACCEPTED, with the ACK bit used to distinguish between those two possibilities.
	- The **FIN** bit is used to release a connection.
		- It specifies that the sender has no more data to transmit.
- The **Window size** field tells how many bytes may be sent starting at the byte acknowledged.
- A **Checksum** is also provided for extra reliability.
	- It checksums the header, the data, and a conceptual pseudoheader in exactly the same way as [[UDP (User Datagram Protocol)|UDP]], except that the pseudoheader has the protocol number for TCP (6) and the checksum is mandatory.
- The **Options** field provides a way to add extra facilities not covered by the regular header.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.5.3
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.5.4
