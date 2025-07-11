# Explanation
- *Chain Replication (CR)*: a form of [[Replication#Replicated State Machine|replicated state machine]] for replicating data at the [[Replication#^518c94|application level]] across multiple nodes to provide a strongly consistent storage interface.

## Mechanism
- The basic approach organizes all nodes storing an object in a chain.
	- Nodes form a chain of a defined length.
	- The chain tail handles all read requests, and the chain head handles all write requests.
- **Writes** propagate down the chain in order and are considered **committed** once they reach the tail.
	- When a write operation is received by a node, it is propagated to the next node in the chain.
	- Once the write reaches the tail node, it has been applied to all replicas in the chain, and it is considered committed.
- **Reads** are only handled by the tail node, so only committed values can be returned by a read.
- CR achieves **strong consistency**/[[Linearizability]] because all writes are committed at the tail and all reads come from the tail.

## Types
- **Basic Chain Replication**: This is the original chain replication method.
	- All reads must be handled by the tail node and writes propagate down the chain from the head.
	- This method has good throughput and easy recovery, but suffers from read throughput limitations and potential hotspots.
- **Chain Replication with Apportioned Queries (CRAQ)**: This method improves upon basic Chain Replication by allowing any node in the chain to handle read operations while still providing strong consistency guarantees.
	- This is achieved by allowing nodes to store multiple versions of an object and using version queries to ensure consistency.

## Advantages
- **Strong Consistency**: CR guarantees that all reads will see the latest written value, ensuring data consistency.
- **Simplicity**: CR is relatively simple to implement compared to other strong consistency protocols.
- **Good Throughput**: Write operations are cheaper than in other protocols offering strong consistency, and can be pipelined down the chain, distributing transmission costs evenly.
	- The head sends each write just once, while in Raft, the leader sends writes to all nodes.
- **Quick and Easy Recovery**: The chain structure simplifies failure recovery, making it faster and easier compared to other methods.
	- Every replica knows of every committed write, and if the head or tail fails, their successor takes over.

## Disadvantages
- **Limited Read Throughput**: All reads must go to the tail node, limiting read throughput to the capacity of a single node.
	- In Raft, reads involve all servers, not just one as in CR.
- **Hotspots**: The single point of read access at the tail can create performance bottlenecks.
- **Increased Latency**: Wide-area deployments can suffer from increased write latency as writes must propagate through the entire chain.
- **Not Immediately Fault-Tolerant**: All CRAQ replicas must participate for any write to commit. If a node is not reachable, CRAQ must wait. This is different from protocols like ZooKeeper and Raft, which are immediately fault-tolerant.
- **Susceptible to Partitions**: Because CRAQ is not immediately fault-tolerant, it is not able to handle partitions. If a partition happens, the system may experience split brain, and must wait until the network is restored.

## Handling Partitions
- To safely use a replication system that can't handle partitions, a single configuration manager must be used.
	- The configuration manager is responsible for choosing the head, chain, and tail of the system.
	- Servers and clients must obey the configuration manager, or stop operating, regardless of which nodes they think are alive or dead.
- A **configuration manager** is a common pattern for distributed systems.
	- It is a core part of systems such as [[The Google File System]] and VMware-FT.
	- Typically, Paxos, Raft, or ZooKeeper are used as the configuration service.

## Addressing Limitations
- One way to improve read throughput in CR is to split objects over multiple chains, where each server participates in multiple chains.
- This only works if the load is evenly distributed among the chains.
- However, load is often not evenly distributed, meaning that splitting objects over multiple chains may not be a viable solution.
- CRAQ offers another way to address the read throughput limitation of CR.

# Sources
- [MIT 6.824 - Lecture 9](https://www.youtube.com/watch?v=IXHzbCuADt0)
