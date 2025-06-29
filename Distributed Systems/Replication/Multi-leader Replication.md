# Explanation
- *Multi-leader Replication*: a replication scheme where more than one node is allowed to accept writes.
- In this setup, each leader simultaneously acts as a follower to the other leaders.

## Use Cases

### Multi-datacenter operation
- In a multi-leader configuration, you can have a leader in each datacenter.
- Within each datacenter, regular leader-follower replication is used; between datacenters, each datacenter’s leader replicates its changes to the leaders in other datacenters.
- [[Single-leader Replication]] vs Multi-leader Replication in multi-datacenter deployment: 

|                                     | Single-leader Replication                                                                                                                                                                               | Multi-leader Replication                                                                                                                                                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Performance**                     | Every write must go over the internet to the datacenter with the leader. This can add significant latency to writes and might contravene the purpose of having multiple datacenters in the first place. | Every write can be processed in the local datacenter and is replicated asynchronously to the other datacenters. Thus, the inter-<br>datacenter network delay is hidden from users, which means the perceived performance may be better. |
| **Tolerance of datacenter outages** | If the datacenter with the leader fails, [[Handling Node Outages in Replication#Leader failure Failover\|failover]] can promote a follower in another datacenter to be leader.                          | Each datacenter can continue operating independently of the others, and replication catches up when the failed datacenter comes back online                                                                                             |
| **Tolerance of network problems**   | Very sensitive to problems in the inter-datacenter link, because writes are made synchronously over this link.                                                                                          | with asynchronous replication can usually tolerate network problems better: a temporary network interruption does not prevent writes being processed.                                                                                   |


## Disadvantages
- The same data may be concurrently modified in two different datacenters, and those write conflicts must be resolved.
- There are often subtle configuration pitfalls and surprising interactions with other database features.

# Sources
- Designing Data-Intensive Applications - Chapter 5
