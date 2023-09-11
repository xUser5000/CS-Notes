# Explanation
- It's a scheduling algorithm employed by Linux.
- Mechanism:
	- Each process has a *virtual runtime* (`vruntime`), which is proportional to physical time.
	- As a process runs, it accumulates `vruntime`.
	- On a #context_switch, CFS will pick the process with the lowest `vruntime` to run next.
	- #scheduling_latency: it's a parameter that indirectly determines the time slice for each process.
		- Time Slice = Scheduling Latency / N, where N is the number of ready processes in the system.
	- If the time slice < #min_granularity, CFS will select #min_granularity as time slice.
	- The #niceness value of a process determines the priority of that process.
		- Low #niceness means high chance of running and vice versa.
		- #niceness $\in [-20, +19]$
		- #niceness can be set by the system users or administrators. 
	- CFS maps each #niceness value to a weight using a pre-defined table.
	- Weights are then used to calculate a suitable time slice for each process.
		- $time\ slice = \frac{weight_k}{\sum_{i=0}^{n-1} weight_i} \cdot sched\ latency$
	- CFS makes use of a *red-black tree* to get the process with lowest `vruntime` in `O(log N)`.

# Sources
- OSTEP Chapter 9 - "Scheduling: Proportional share"