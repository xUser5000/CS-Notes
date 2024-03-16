# Explanation
- The transport layer builds on the [[Network Layer]] to provide data transport from a process on a source machine to a process on a destination machine with a desired level of reliability that is independent of the physical networks currently in use.
	- It provides a reliable service on top of an unreliable network.
	- It provides the abstractions that applications need to use the network.
	- The transport code runs entirely on the users’ machines, but the network layer mostly runs on the routers.
- *Transport Entity*: software and/or hardware within the transport layer that does the work.
	- It can be located in the [[Operating Systems|operating system]]'s kernel, a library package bound into network applications, in a separate users [[Process|process]], or even on the network interface card.
- *Segment*: messages sent from transport entity to transport entity.
- Segments (exchanged by the transport layer) are contained in packets (exchanged by the [[Network Layer]]), which are contained in frames (exchanged by the [[Data Link Layer]]).
	- Illustration: ![[Nesting of segments, packets, and frames.png]]
- The Internet has two main protocols in the transport layer that complement each other:
	- [[UDP (User Datagram Protocol)]]: a connection-less protocol that does nothing beyond sending packets between applications.
	- [[TCP]]: a connection-oriented protocol that adds reliability with retransmissions, along with flow control and congestion control.

## Transport Service Primitives
- The transport layer, just like the network layer, provides two types of services:
	- **Connection-oriented Services**
	- **Connection-less Services**

# Topics
- Elements of transport protocols:
	- [[Addressing]]
	- [[Connection Establishment]]
	- [[Connection Release]]
	- [[Error Control and Flow Control]]
- [[Computer Networks/Transport Layer/Congestion Control|Congestion Control]]

# FAQ
- if the transport layer service is so similar to the network layer service, why are there two distinct layers?
	- The transport layer improves the quality of services presented by the network layer.
	- The existence of the transport layer makes it possible for the transport service to be more reliable than the underlying network.
	- The transport primitives can be implemented as calls to library procedures to make them independent of the network primitives.
		- This ensures that changing the network merely requires replacing one set of library procedures with another one that does the same thing with a different underlying service.
		- Application programmers can write code according to a standard set of primitives and have these programs work on a wide variety of networks, without having to worry about dealing with different network interfaces and levels of reliability.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.1
