# Explanation
- *Link State Routing*: a routing algorithm where the complete topology is distributed to every router, then Dijkstra's shortest-path algorithm can be run at each router to find the shortest path to every other router.
- Each router must do the following to make it work:

## Mechanism

### 1. Learning about the neighbors
- When a router is booted, its first task is to learn who its neighbors are.
- It accomplishes this goal by sending a special HELLO packet on each point-to-point line.
- The router on the other end is expected to send back a reply giving its name.
- The names must be globally unique.

### 2. Setting Link Costs
- The link state routing algorithm requires each link to have a distance or cost metric for finding shortest paths.
- The cost to reach neighbors can be set automatically, or configured by the network operator.
- A common choice is to make the cost inversely proportional to the bandwidth of the link.

### 3. Building Link State Packets
- Once the information needed for the exchange has been collected, the next step is for each router to build a packet containing all the data.
- The packet starts with the identity of the sender, followed by a sequence number and age, a list of neighbors, and the cost to each neighbor.
- When to build link state packets? either
	- periodically, or
	- when some significant event occurs, such as a line or neighbor going down or coming back up again or changing its properties.

### 4. Distributing the Link State Packets
- The fundamental idea is to use [[Flooding|flooding]] to distribute the link state packets to all routers.
- To keep the flood in check, routers use sequence numbers to uniquely identify packets.
	- If a packet with a sequence number lower than the highest one seen so far arrives, it is rejected as being obsolete as the router has more recent data.
- To guard against errors on the links, all link state packets are acknowledged.

#### Possible Optimization
- When a link state packet comes in, it's not queued for transmission immediately.
- Instead, it's put in a holding area to wait for a short while in case more links are coming up or going down.
- If another link state packet from the same router source comes in before the first packet is transmitted, their sequence numbers are compared and the older one is thrown out.


#### Problems

| Problem                                                                                                                                                                                                                                                                                                                          | Solution                                                                                                                                                                                                   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. If the sequence numbers wrap around, there will be confusion.                                                                                                                                                                                                                                                                 | The solution is to use 32-bit sequence numbers and send one link state packet per second. It would take 137 years to wrap around.                                                                          |
| 2. If a router ever crashes, it will lose track of its sequence number. If it starts again at 0, the next packet sent will be rejected as a duplicate by other routers.<br>3. If a sequence number is ever corrupted and 65,540 is received instead of 4 (a 1-bit error), packets 5 through 65,540 will be rejected as obsolete. | The solution to all these problems is to include the age of each packet after the sequence number and decrement it once per second. When the age hits zero, the information from that router is discarded. |
|                                                                                                                                                                                                                                                                                                                                  |                                                                                                                                                                                                            |

### 5. Computing the New Routes
- Once a router has accumulated a full set of link state packets, it can construct the entire network graph because every link is represented.
	- In fact, every link is represented twice, once for each direction.
	- The different directions may even have different costs.
- Now Dijkstra’s algorithm can be run locally to construct the shortest paths to all possible destinations.
	- The results of the algorithm are installed in the routing tables to tell the router which link to use to reach each destination.

## Advantages
- Does not suffer from slow convergence problems.
- Widely used in actual networks.

## Disadvantages
- Requires more memory and computation than [[Distance Vector Routing]].
	-  For a network with $n$ routers, each of which has $k$ neighbors, the memory required to store the input data is proportional to $k \cdot n$.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.2.5
