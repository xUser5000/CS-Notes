# Explanation
- A distributed transaction accesses data at one or more partitions, which requires expensive coordination.

## Centralized coordinator
- The centralized coordinator acts as a global “traffic cop” that coordinates all the behavior.
- Illustration: ![[Centralized Coordinator.png]]

## Middleware
- Centralized coordinators can be used as middleware, which accepts query requests and routes queries to correct partitions.

## Decentralized coordinator
- In a decentralized approach, nodes organize themselves.
- The client directly sends queries to one of the partitions.
- This home partition will send results back to the client.
	- The home partition is in charge of communicating with other partitions and committing accordingly.

# Sources
- CMU 15-445 Lecture 21 - "Introduction to Distributed Databases"