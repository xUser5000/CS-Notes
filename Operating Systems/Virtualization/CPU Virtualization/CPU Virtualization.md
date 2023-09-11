# Explanation
- It's the abstraction created by the OS such that each [[Process]] runs as if it has its own CPU.
- The OS creates such an illusion by #time_sharing the CPU.
	- It runs one process for some time, then run another one, and so forth.
- To implement #time_sharing in a manner that reduces overheads and retains control, the OS makes use of [[Limited Direct Execution]].
- In a #context_switch, the OS must decides which process/job to run next based on a pre-determined [[Scheduling Algorithm]].

# Sources
- Extends [[Virtualization#Sources]]