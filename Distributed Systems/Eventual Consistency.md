# Explanation
- *Eventual Consistency*: a consistency model that guarantees that if you stop writing to the [[Database Systems|database]] and wait for some unspecified length of time, then eventually all read requests will return the same value.
- In other words, the inconsistency is temporary, and it eventually resolves itself.
- A better name for eventual consistency may be convergence, as we expect all replicas to eventually converge to the same value.

# Sources
- Designing Data-Intensive Applications - Chapter 9