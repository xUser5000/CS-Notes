# Explanation
- *Load shedding*: a fancy way of saying that when routers are being inundated by packets that they cannot handle, they just throw them away.
- It's usually used when none of the [[Computer Networks/Network Layer/Congestion Control#Approaches to Congestion Control|congestion control approaches]] work.

## Which packets to drop?
- The preferred choice may depend on the type of applications that use the network.
	- For a file transfer, an old packet is worth more than a new one.
	- For real-time media, a new packet is worth more than an old one.
- More intelligent load shedding requires cooperation from the senders.
	- **Example**: packets that carry routing information are more important than regular data packets because they establish routes;
		- if they are lost, the network may lose connectivity.
- To implement an intelligent discard policy, applications must mark their packets to indicate to the network how important they are.
	- Then, when packets have to be discarded, routers can first drop packets from the least important class, then the next most important class, and so on.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.3.5
