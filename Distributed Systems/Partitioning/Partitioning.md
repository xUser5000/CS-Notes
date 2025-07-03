# Explanation
- *Partitioning*: breaking the data up into partitions, also known as sharding.
- Partitions are defined in such a way that each piece of data (each record, row, or document) belongs to exactly one partition.
- The main reason for wanting to partition data is scalability: different partitions can be placed on different nodes in a [[Distributed Database Architecture#Shared Nothing|shared-nothing]] cluster.
	- A large dataset can be distributed across many disks, and the query load can be distributed across many processors.

## Partitioning & Replication
- Partitioning is usually combined with [[Replication|replication]] so that copies of each partition are stored on multiple nodes.
- Even though each record belongs to exactly one partition, it may still be stored on several different nodes for fault tolerance.
- A node may store more than one partition.
- Each partition’s leader is assigned to one node, and its followers are assigned to other nodes.
- Each node may be the leader for some partitions and a follower for other partitions.
- The choice of partitioning scheme is mostly independent of the choice of replication scheme.
- Illustration: ![[Partitioning & Replication.png]]

## Partitioning Schemes
- The goal with partitioning is to spread the data and the query load evenly across nodes.
	- If every node takes a fair share, then—in theory—10 nodes should be able to handle 10 times as much data and 10 times the read and write throughput of a single node .
- *Skew*: a case where the partitioning is unfair, so that some partitions have more data or queries than others, which makes partitioning much less effective.
- *Hot Spot*: A partition with disproportionately high load.

### Random Partitioning
- **Mechanism**: the simplest approach for avoiding hot spots would be to assign records to nodes randomly.
- **Advantage**: distributes the data quite evenly across the nodes.
- **Disadvantage**: when you’re trying to read a particular item, you have no way of knowing which node it is on, so you have to query all nodes in parallel.

### Partitioning by Key Range
- **Mechanism**: 
	- Assign a continuous range of keys (from some minimum to some maximum) to each partition.
	- If you know the boundaries between the ranges, you can easily determine which partition contains a given key.
	- If you also know which partition is assigned to which node, then you can make your request directly to the appropriate node.
	- The ranges of keys are not necessarily evenly spaced, because your data may not be evenly distributed.
		- In order to distribute the data evenly, the partition boundaries need to adapt to the data.
- **Advantage**: Efficient range queries.
- - **Disadvantage**: certain access patterns can lead to hot spots (e.g., partitioning time-series data based on the timestamp range).

### Partitioning by Hash of Key
-  **Mechanism**: determine the partition of each key using a hash function.
	- A good hash function takes skewed data and makes it uniformly distributed.
	- For partitioning purposes, the hash function need not be cryptographically strong.
	- Once you have a suitable hash function for keys, you can assign each partition a range of hashes (rather than a range of keys), and every key whose hash falls within a partition’s range will be stored in that partition.
	- The partition boundaries can be evenly spaced, or they can be chosen pseudorandomly (in which case the technique is sometimes known as consistent hashing).
- **Advantage**: good at distributing keys fairly among the partitions.
- **Disadvantage**: we lose a nice property of key-range partitioning: the ability to do efficient range queries.
	- Keys that were once adjacent are now scattered across all the partitions, so their sort order is lost.

## Rebalancing Partitions
- *Rebalancing*: The process of moving load from one node in the cluster to another.
- Rebalancing is needed to react to changes in the cluster like scaling up/down nodes, increasing dataset size, machine failures, etc...
- No matter which partitioning scheme is used, rebalancing is usually expected to meet some minimum requirements:
	- After rebalancing, the load (data storage, read and write requests) should be shared fairly between the nodes in the cluster.
	- While rebalancing is happening, the database should continue accepting reads and writes.
	- No more data than necessary should be moved between nodes, to make rebalancing fast and to minimize the network and disk I/O load.

### Rebalancing Strategies

#### Hash mod $N$
- The problem with the mod $N$ approach is that if the number of nodes N changes, most of the keys will need to be moved from one node to another.

#### Fixed number of partitions
- **Mechanism**:
	- Create many more partitions than there are nodes, and assign several partitions to each node.
	- If a node is added to the cluster, the new node can steal a few partitions from every existing node until partitions are fairly distributed once again.
	- If a node is removed from the cluster, the same happens in reverse.
	- Only entire partitions are moved between nodes.
	  The number of partitions does not change, nor does the assignment of keys to partitions.
	- The only thing that changes is the assignment of partitions to nodes.
- **Advantages**:
	- You can account for mismatched hardware in your cluster: by assigning more partitions to nodes that are more powerful, you can force those nodes to take a greater share of the load.
- **Disadvantages**:
	- The best performance is achieved when the size of partitions is “just right,” neither too big nor too small, which can be hard to achieve if the number of partitions is fixed but the dataset size varies.
		- If partitions are very large, rebalancing and recovery from node failures become expensive.
		- If partitions are too small, they incur too much operational overhead.

#### Dynamic Partitioning
- **Mechanism**:
	- When a partition grows to exceed a configured size, it is split into two partitions so that approximately half of the data ends up on each side of the split.
	- Conversely, if lots of data is deleted and a partition shrinks below some threshold, it can be merged with an adjacent partition.
	- After a large partition has been split, one of its two halves can be transferred to another node in order to balance the load.
- **Caveat**: An empty database starts off with a single partition, since there is no a priori information about where to draw the partition boundaries. While the dataset is small, all writes have to be processed by a single node while the other nodes sit idle.
	- To mitigate this issue, HBase and MongoDB allow an initial set of partitions to be configured on an empty database (this is called *pre-splitting*).
- **Advantages**:
	-  The number of partitions adapts to the total data volume.
- **Disadvantages**:

#### Partitioning proportionally to nodes
- **Mechanism**:
	- Make the number of partitions proportional to the number of nodes—in other words, to have a fixed number of partitions per node.
	- In this case, the size of each partition grows proportionally to the dataset size while the number of nodes remains unchanged, but when you increase the number of nodes, the partitions become smaller again.
	- Since a larger data volume generally requires a larger number of nodes to store, this approach also keeps the size of each partition fairly stable.
	- When a new node joins the cluster, it randomly chooses a fixed number of existing partitions to split, and then takes ownership of one half of each of those split partitions while leaving the other half of each partition in place.
- **Advantages**:
- **Disadvantages**:

# Sources
- Designing Data-Intensive Applications - Chapter 6
