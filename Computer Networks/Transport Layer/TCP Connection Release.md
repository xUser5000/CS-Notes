# Explanation
- A TCP connections is best thought of as a pair of simplex connections, each one is released independently of its sibling.

## Mechanism
- To release a connection, either party can send a TCP segment with the FIN bit set, which means that it has no more data to transmit.
- When the FIN is acknowledged, that direction is shut down for new data. 
	- Data may continue to flow indefinitely in the other direction, however.
- When both directions have been shut down, the connection is released.
- Normally, four TCP segments are needed to release a connection: one FIN and one ACK for each direction.
	- However, it is possible for the first ACK and the second FIN to be contained in the same segment, reducing the total count to three.
- Both ends of a TCP connection may send FIN segments at the same time.
	- In this case, segments are each acknowledged in the usual way, and the connection is shut down.
	- There is no essential difference between the two hosts releasing sequentially or simultaneously.
- To avoid the [[Connection Release#The Two-Army Problem|two army problem]], timers are used.
	- If a response to a FIN is not forthcoming within two maximum packet lifetimes, the sender of the FIN releases the connection.
	- The other side will eventually notice that nobody seems to be listening to it anymore and will time out as well.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.5.6
