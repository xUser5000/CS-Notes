# Explanation
- *Distance Vector Routing*: a routing algorithm that operates by having each router maintain a table (i.e., a vector) giving the best known distance to each destination and which link to use to get there.
	- Also known as the **Distributed Bellman-Ford Routing Algorithm**.
- Routing tables are updated by exchanging information with neighbors and, eventually, every router knows the best link to reach each destination.
- The distance might be measured as the number of hops or using another metric.
- Each router is assumed to know the distance to each of its neighbors.
	- If the metric is hops, the distance is just one hop.
	- If the metric is propagation delay, the router sends a special ECHO packet that the receiver just timestamps and sends back ASAP.

## Advantages
- A simple technique by which routers can collectively compute shortest paths.

## Disadvantages
- Converges to the correct answer slowly.
- Reacts rapidly to good news, but leisurely to bad news.
	- Due to the **Count-to-Infinity Problem**.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.2.4
