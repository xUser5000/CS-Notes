# Explanation
- In large networks, routing must be done hierarchically.
- As networks grow in size, the routing tables grow proportionally until it is no longer feasible for every router to have an entry for every other router.

## Mechanism
- Routers are divided into so-called **regions**.
- Each router knows all the details about how to route packets to destinations within its own region but knows nothing about the internal structure of other regions.
	- All other regions are condensed into a single router.
- When different networks are interconnected, it is natural to regard each one as a separate region to free the routers in one network from having to know the topological structure of the other ones.
- It may be necessary to group regions int clusters, the clusters into zones, the zones into groups, and so on.
- As the ration of the number of regions to the number or routers per regions grows, the savings in the table space increase.

## Advantages
- Scales well in large networks.

## Disadvantages
- Results in increased path length.
	- However, the increase in effective mean path length is sufficiently small that it's usually acceptable.

# FAQ
- How many levels should the hierarchy have?
	- The optimal number of levels for an $N$ router network is $ln(N)$, requiring a total of $e \cdot ln(N)$ entries per router.

# Sources 
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.2.6
