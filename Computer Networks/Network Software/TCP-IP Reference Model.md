# Explanation
- It's the reference layer used in the internet.
- Unlike the [[OSI Reference Model]], the TCP/IP model does not have session or presentation layers.

## Illustration
![[The TCP-IP Reference Model.png]]

## Layers
![[The TCP-IP Reference Model with some protocols.png]]

### The Link Layer
- The link layer describes what links such as serial lines and classic Ethernet must do to meet the needs of this connectionless internet layer.

### The Internet Layer
- The job of the internet layer is to permit hosts to inject packets into any network and have them travel independently to the destination (potentially on a different network).
- The packets may even arrive in a completely different order than they were sent, in which case it is the job of higher layers to rearrange them, if in-order delivery is desired.
- The internet layer defines an official packet format and protocol called IP (Internet Protocol).

### The Transport Layer
- The transport layer is designed to allow peer entities on the source and destination hosts to carry on a conversation.
	- Similar to the transport layer in the [[OSI Reference Model]].
- Protocols:
	- [[UDP (User Datagram Protocol)]]
	- [[TCP]]

### The Application Layer
- The application layer sits on top of the transport layers and contains all the higher-level protocols.
- Protocols:
	- [[HTTP]]
	- [[SMTP]]
	- [[DNS]]
	- FTP
	- RTP
	- TELNET

## Advantages
- The protocols came before the model and the model was just a description of the already-existing protocols, so they fit perfectly

## Disadvantages
- The model didn't fit any other protocol stack.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 1.4.2