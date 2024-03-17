# Explanation
- Goals of a congestion control algorithm:
	- Avoid congestion
	- Find a good allocation of bandwidth to the transport entities that are using the network.
		- A "good allocation" will deliver good performance because it uses all the available bandwidth but avoids congestion, it will be fair across competing transport entities, and it will quickly track changes in traffic demands.

## Efficiency and Power
- An efficient allocation of bandwidth across transport entities will use all of the network capacity that is available.
- However, it is not quite right to think that if there is a 100-Mbps link, five transport entities should get 20 Mbps each.
	- They should usually get less than 20 Mbps for good performance.
- For both goodput and delay, performance begins to degrade at the onset of congestion.
- We will obtain the best performance from the network if we allocate bandwidth up until the delay starts to climb rapidly.
	- To identify such point, we can use the metric of **power**.

$$power = \frac{load}{delay}$$
- Power will initially rise with offered load, as delay remains small and roughly constant, but will reach a maximum and fall as delay grows rapidly.
- The load with the highest power represents an efficient load for the transport entity to place on the network.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.3.1
