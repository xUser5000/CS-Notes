# Explanation
- It's a [[Scheduling Algorithm]] that has the following goals:
	- optimizing the #turnaround time, by running shorter jobs first.
	- making the system feel responsive for interactive users.
- Characteristics:
	- It has a number of distinct queues, each assigned a different priority level.
	- At any given time, a job that is ready to run is on a single queue.
	- It uses feedback to determine the priority of each job.
	- Most MLFQ variants allow for varying time-slice length across different queues.
- Mechanism: for two jobs A and B,
	1. If Priority(A) > Priority(B), A runs (B doesn't).
	2. If Priority(A) = Priority(B), A & B run in round-robin fashion using the time slice of the given queue.
	3. When a job enters the system, it's placed at the highest priority (topmost queue).
	4. Once a job uses up its time allotment at a given level (regardless of how many times it has given up the CPU), its priority is reduced (i.e., it moves down one queue).
	5. After some time period S, move all the jobs in the system to the topmost queue.

# Sources
- OSTEP Chapter 8 - "Scheduling: The Multi-Level Feedback Queue"