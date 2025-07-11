# Explanation
- The consensus problem is normally formalized as follows: one or more nodes may propose values, and the consensus algorithm decides on one of those values.
- The best-known fault-tolerant consensus algorithms are Viewstamped Replication (VSR), Paxos, Raft, and Zab (used in Zookeeper).

## Properties
- A consensus algorithm must satisfy the following properties:
	- **Uniform agreement**: No two nodes decide differently.
	- **Integrity**: No node decides twice.
	- **Validity**: If a node decides value v, then v was proposed by some node.
	- **Termination**: Every node that does not crash eventually decides some value.

- The *uniform agreement* and *integrity* properties define the core idea of consensus: everyone decides on the same outcome, and once you have decided, you cannot change your mind.
- The *validity* property exists mostly to rule out trivial solutions: for example, you could have an algorithm that always decides null, no matter what was proposed; this algorithm would satisfy the agreement and integrity properties, but not the validity property.
- The termination property formalizes the idea of fault tolerance. It essentially says that a consensus algorithm cannot simply sit around and do nothing forever (i.e., it must make progress).
	- Even if some nodes fail, the other nodes must still reach a decision.
- Termination is a liveness property, whereas the other three are safety properties.

## System Model
- The system model of consensus assumes that when a node “crashes,” it suddenly disappears and never comes back.
-  In this system model, any algorithm that has to wait for a node to recover is not going to be able to satisfy the termination property.
	- In particular, [[Two-Phase Commit|2PC]] does not meet the requirements for termination.
- If all nodes crash and none of them are running, then it is not possible for any algorithm to decide anything.
	- There is a limit to the number of failures that an algorithm can tolerate.
	- It can be proved that any consensus algorithm requires at least a majority of nodes to be functioning correctly in order to assure termination.
	- This majority can safely form a [[Replication Quorums|quorum]].

- The termination property is subject to the assumption that fewer than half of the nodes are crashed or unreachable.
- Most implementations of consensus ensure that the safety properties—agreement, integrity, and validity—are always met, even if a majority of nodes fail or there is a severe network problem.
	- Thus, a large-scale outage can stop the system from being able to process requests, but it cannot corrupt the consensus system by causing it to make invalid decisions.

- Most consensus algorithms assume that there are no [[Byzantine Faults|Byzantine faults]].
- It is possible to make consensus robust against Byzantine faults as long as fewer than one-third of the nodes are Byzantine-faulty.

## [[Single-leader Replication|Single-leader replication]] and consensus
- If the leader is manually chosen and configured by by a human, we essentially have a “consensus algorithm” of the dictatorial variety: only one node is allowed to accept writes (i.e., make decisions about the order of writes in the replication log).
- If the leader goes down, the system becomes unavailable for writes until the operators manually configure a different node to be the leader.
- Such a system can work well in practice, but it does not satisfy the termination property of consensus because it requires human intervention in order to make progress.

## Epoch numbering and quorums
- All of the consensus protocols discussed so far internally use a leader in some form or another, but they don’t guarantee that the leader is unique.
- Instead, they can make a weaker guarantee: the protocols define an epoch number and guarantee that within each epoch, the leader is unique.
- Every time the current leader is thought to be dead, a vote is started among the nodes to elect a new leader.
- Such election is given an incremented epoch number, and thus epoch numbers are totally ordered and monotonically increasing.
- If there is a conflict between two different leaders in two different epochs then the leader with the higher epoch number prevails.
- Before a leader is allowed to decide anything, it must first check that there isn’t some other leader with a higher epoch number which might take a conflicting decision.
- Just because a node thinks that it is the leader, that does not necessarily mean the other nodes accept it as their leader.
	- Instead, it must collect votes from a quorum of nodes.
- For every decision that a leader wants to make, it must send the proposed value to the other nodes and wait for a quorum of nodes to respond in favor of the proposal.
- The quorum typically, but not always, consists of a majority of nodes.
- A node votes in favor of a proposal only if it is not aware of any other leader with a higher epoch.

## Limitations
- The process by which nodes vote on proposals before they are decided is a kind of synchronous replication.
	- In this configuration, some committed data can potentially be lost on failover but many people choose to accept this risk for the sake of better performance.
- **Consensus systems always require a strict majority to operate**.
	-  If a network failure cuts off some nodes from the rest, only the majority portion of the network can make progress, and the rest is blocked.
- **Most consensus algorithms assume a fixed set of nodes that participate in voting,** **which means that you can’t just add or remove nodes in the cluster.**
	- Dynamic membership extensions to consensus algorithms allow the set of nodes in the cluster to change over time, but they are much less well understood than static membership algorithms.
- Consensus systems generally rely on timeouts to detect failed nodes. In environments with highly variable network delays, especially geographically distributed systems, **it often happens that a node falsely believes the leader to have failed due to a transient network issue.**
	- Although this error does not harm the safety properties, frequent leader elections result in terrible performance because the system can end up spending more time choosing a leader than doing any useful work.
- Sometimes, consensus algorithms are particularly sensitive to network problems.

# Sources
- Designing Data-Intensive Applications - Chapter 9