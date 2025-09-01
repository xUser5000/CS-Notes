# Explanation
- When something unexpected occurs during packet processing at a router, the event is reported to the sender by the ICMP (Internet Control Message Protocol).
- ICMP is also used to test the Internet.
- Each ICMP message type is carried encapsulated in an [[Internet Protocol (IP)|IP]] packet.
- Important Message Types:
	- The **DESTINATION UNREACHABLE** message is used when the router cannot locate the destination.
	- The **TIME EXCEEDED** message is sent when a packet is dropped because its TtL (Time to live) counter has reached zero.
		- This event is a symptom that packets are looping, or that the counter values are being set too low.
	- The **PARAMETER PROBLEM** message indicates that an illegal value has been detected in a header field.
		- This problem indicates a bug in the sending host’s IP software or possibly in the software of a router transited.
	- The **SOURCE QUENCH** message was long ago used to throttle hosts that were sending too many packets.
		- It is rarely used anymore because when congestion occurs, these packets tend to add more fuel to the fire.
	- The **REDIRECT** message is used when a router notices that a packet seems to be routed incorrectly.
	- The **ECHO** and **ECHO REPLY** messages are sent by hosts to see if a given destination is reachable and currently alive.
		- Upon receiving the ECHO message, the destination is expected to send back an ECHO REPLY message.
		-  These messages are used in the ping utility that checks if a host is up and on the Internet.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.6.4
