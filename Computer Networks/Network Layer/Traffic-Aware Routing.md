# Explanation
- Regular [[Routing Algorithms|routing algorithms]] used fixed link weights and only adapted to changes in the topology, but no to changes in load.
- The goal in Taking load into account when computing routes is to shift traffic away from hotspots that will be the first places in the network to experience congestion.

## Mechanism
- We can set the link weight to be a function of the (fixed) link bandwidth and propagation delay plus the (variable) measured load or average queuing delay.
- Least-weight paths will then favor paths that are more lightly loaded, all else being equal.

## Problem
![[A network in which the East and West parts are connected by two links.png]]
- Suppose that most of the traffic between East and West is using link $CF$, and, as a result, this link is heavily loaded with long delays.
- Including queueing delay in the weight used for the shortest path calculation will make $EI$ more attractive.
- After the new routing tables have been installed, most of the East-West traffic will now go over $EI$, loading this link.
- Consequently, in the next update, $CF$ will appear to be the shortest path. As a result, the routing tables may oscillate wildly, leading to erratic routing and many potential problems.

### Solution
- Use **Multipath routing** in which there can be multiple paths from a source to a destination.
	- This means that traffic can be spread across multiple links.
- Shift traffic across routes slowly enough that it is able to converge.

# Sources 
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.3.2
