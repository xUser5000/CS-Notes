# Explanation
- *Flooding*: a routing algorithm in which every incoming packet is sent out on every outgoing link except the one it arrived on.
- It generates an infinite number of duplicate packets, which can be solved by either of the following:
	- **Hop Counter**: inject a hop counter in the header of each packet that is decremented at each hop, with the packet being discarded when the counter reaches zero.
	- **Sequence Numbers**: have routers keep track of which packets have been flooded to avoid sending them a second time.

## Advantages
- Ensures that a packet is delivered to every node in the network
	- Effective for broadcasting information
- Tremendously robust
	- Even if a large number of routers go down, flooding will find a path if one exists.
- Requires little in the way of setup.
	- The routers only need to know their neighbors.

## Disadvantages
- Not practical in most cases.
- Wasteful of resources.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.2.3