# Explanation
- *Routing Algorithm*: the part of the [[Network Layer]] software responsible for deciding which output line an incoming packet should be transmitted on.
	- In case of a datagram network (refer to [[Connection-less Service|Connection-less Services]]), this decision is made for every arriving data packet since the best route may have changed since last time.
	- In case of a virtual-circuit network (refer to [[Connection-oriented Service|Connection-oriented Services]]), this decision is made only when a new virtual circuit is being set up.
		- Also known as **Session Routing** because a route remains in force for an entire session (e.g., while logged in over a VPN).
- Routers can be though of as having two separate processes inside them:
	- **Forwarding**: handles each packet as it arrives, looks up the outgoing line in the routing table, and then forward it to the corresponding line.
	- **Routing**: periodically fills and updates the routing tables to be used in the forwarding process according to the routing algorithm.
- When a routing algorithm is implemented, each router must take decisions based on local knowledge, not the complete picture of the network.
- *Convergence*: the settling of routes to best paths across the network.

## Design Goals
- Correctness
- Simplicity
- Robustness
	- The routing algorithm should be able to cope with changes in the topology and traffic without requiring all jobs in all hosts to be aborted.
- Stability
	- A stable algorithm should reach equilibrium and stays there.
	- It should converge quickly too, since communication may be disrupted until the routing algorithm has reached equilibrium.
- Fairness
- Efficiency

## Classes

### Non-adaptive Algorithms (AKA Static Routing)
- Non-adaptive algorithms do not base their routing decisions on any measurements or estimates of the current topology and traffic.
- The choice of the route from $i$ to $j$ (for all $i$ and $j$) is computed in advance, off-line, and downloaded to the routers when the network is booted.
- Only useful for situations in which the routing choice is clear. 

### Adaptive Algorithms (AKA Dynamic Routing)
- Adaptive algorithms change their routing decisions to reflect changes in the topology, and sometimes changes in the traffic.
- They differ in where they get their information, when they change the routes, and what metric is used for optimization.

## The Optimality Principle
- **Statement**: If router $j$ is on the optimal path from router $i$ to router $k$, the the optimal path from $j$ to $k$ also falls along the same route.
- **Proof**:
	1. Let $r_1$ be the route from $i$ to $j$
	2. Let $r_2$ be the route from $j$ to $k$
	3. If a route better than $r_2$ existed from $j$ to $k$, it could be concatenated with $r_1$ to improve the route from $i$ to $k$, which contradicts our original statement that $r_1$$r_2$ is optimal.
- **Consequence**: the set of optimal routes from all sources to a given destination form a tree rooted at the destination. Such a tree is called a **Sink Tree**.
	- Illustration: ![[Sink Tree.png]]

## Examples
- [[Flooding]]
- [[Distance Vector Routing]]
- [[Link State Routing]]
- [[Hierarchical Routing]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.2
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.2.3
