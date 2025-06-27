# Explanation

## Follower failure: Catch-up recovery
- On its local disk, each follower keeps a log of the data changes it has received from the leader.
- If a follower crashes and is restarted, the follower can recover quite easily:
	1. from its log, it knows the last transaction that was processed before the fault occurred.
	2. Thus, the follower can connect to the leader and request all the data changes that occurred during the time when the follower was disconnected. 
	3. When it has applied these changes, it has caught up to the leader and can continue receiving a stream of data changes as before.

## Leader failure: Failover
- Handling a failure of the leader is trickier: one of the followers needs to be promoted to be the new leader, clients need to be reconfigured to send their writes to the new leader, and the other followers need to start consuming data changes from the new leader.
	- This process is called *failover*.
- Failover can happen manually or automatically.
- Failover is fraught with things that can go wrong:
	- If asynchronous replication is used, the new leader may not have received all the writes from the old leader before it failed.
		- The most common solution is for the old leader’s unreplicated writes to simply be discarded, which may violate clients’ durability expectations.
	- *Split Brain*: two nodes both believe that they are the leader and both accept writes, and there's not process for resolving conflicts, data is likely lost or corrupted.
		- As a safety catch, some systems have a mechanism to shut down one node if two leaders are detected.
	- A longer timeout means a longer time to recovery in the case where the leader fails. However, if the timeout is too short, there could be unnecessary failovers.

# Sources
- Designing Data-Intensive Applications - Chapter 5
