# Explanation
- It's called the ISO OSI (Open Systems Interconnection) Reference Model because it deals with connecting systems that are open for communication with other systems.
- It has seven layers.
- The principles that were applied to arrive at the seven layers are:
	- A layer should be created where a different abstraction is needed.
	- Each layer should perform a well-defined function.
	- The function of each layer should be chosen with an eye toward defining internationally standardized protocols.
	-  The layer boundaries should be chosen to minimize the information flow across the interfaces.
	- The number of layers should be large enough that distinct functions need not be thrown together in the same layer out of necessity and small enough that the architecture does not become unwieldy.

## Illustration
![[OSI Reference Model.png]]

## Layers
- **The Physical Layer** is concerned with transmitting raw bits over a communication channel.
- **The Data Link Layer** transforms a raw transmission facility into a line that appears free of undetected transmission errors.
	- The sender breaks up the data into data frames and transmit the frames sequentially.
	- The receiver confirms correct receipt of each frame by sending back an acknowledgement frame.
	- One issues that arises in this layer is how to keep a fast transmitter from drowning a slow receiver.
- **The Network Layer** controls how packets are routed from source to destination.
	- Handling congestion is another responsibility of the network layer.
	- Allows heterogeneous networks to be interconnected. 
- **The Transport Layer** accepts data from above it, split it up into smaller units if need be, pass these to the network layer, and ensure that the pieces all arrive correctly at the other end.
- **The Session Layer** allows users on different machines to establish sessions between them.
- **The Presentation Layer** is concerned with the syntax and semantics of the information transmitted.
- **The Application Layer** contains a variety of protocols that are commonly needed by users.
	- **Example**: HyperText Transfer Protocol (HTTP).

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 1.4