# Explanation
- *Disk Scheduler*: OS component that examines disk requests and decides which one to schedule next.
- It estimates the time taken for each request based on the seek and rotational delay.

## Shortest Seek Time First (SSTF)
- **Description**: Picks the requests with the nearest track to complete first.
- **Advantages**: 
- **Disadvantages**:
	- The drive geometry is not available for the OS; it sees an array of blocks.
		- Can be solved by implementing Nearest Block First (NBF).
	- Starvation: requests to faraway tracks starve if there is a steady stream of tracks near the head.

## Elevator (SCAN)
- **Description**:
	- Moves back and forth across tracks servicing requests.
	- *Sweep*: a single pass across the disk.
	- If a request comes for a block on a track that has already been services on this sweep, it is not handles immediately until the next sweep.
- **Advantages**:
	- does not cause starvation.
- **Disadvantages**:
	- favors middle tracks.
	- Doesn't adhere closely to the principles of SJF ([[Scheduling Algorithm#^d1a8c2]])

## C-SCAN
- **Description**:
	- Extends [[#Elevator (SCAN)]] description.
	- Only sweeps from outer-to-inner, then resets at the outer tracks to begin again.
- **Advantages**:
	- Extends [[#Elevator (SCAN)]] advantages.
	- More fair to inner and outer blocks.
- **Disadvantages**:
	- Doesn't adhere closely to the principles of SJF ([[Scheduling Algorithm#^d1a8c2]])

## Shortest Positioning Time First (SPTF)
- Takes into account both the seek and rotational latency.
	- They are roughly equivalent on modern drives.
- More difficult to implement in the OS, thus it's usually performed inside the drive.

# Sources
- Extends [[Hard Disk Drives (HDD)#Sources]]