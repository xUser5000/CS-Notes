# Explanation
- It's an electro-mechanical data storage device that stores and retrieves digital data using magnetic storage with one or more rigid rapidly rotating platters coated with magnetic material. [1]

## Interface [2]
- The disk provides an array of sectors numbered from `0` to `n-1`.
- Each sector can be read or written to.
- Multi-sector operations are possible.
	- However, only a single 512-byte write is guaranteed to be #atomic.
- Accessing two blocks near one another is faster than accessing two blocks that are far apart.

## Internal Structure [1]
![[What's inside an HDD.png]]
- *Platter*: a circular hard surface on which data is stored persistently by inducing magnetic changes to it.
	- A disk has multiple platters.
	- Each platter has two sides called *surface*s.
- Platters are bound together around a *spindle*, which is connected to a motor that spins the platters around with a constant rate.
- *Track*: concentric circles of sectors.
	- A surface may have thousands of tracks.
	- Outer tracks tend to have more sectors than inner tracks.
	- Hundreds of tracks can fit into the width of a human hair.
- *Disk Head*: a component responsible for reading and writing data.
	- There is one head per surface.
- *Disk Arm*: moves across the surface to position the head over the desired track.
- *Track Skew*: property exhibited by hard drives to make sure that sequential reads can be properly serviced even when crossing track boundaries. Illustration: ![[Track Skew.png]]
- *Track Buffer*: a small [[Cache]] memory inside the hard drive used to hold data read and written to the disk.
	- Extends [[Cache#Write Behavior]].

## Computing Capacity [1]
![[Disk capacity formula.png]]
- The access time of some target sector = seek time + rotational latency + transfer time.
	- *Seek time*: time to position the head under the target sector (3 - 9 ms).
	- *Rotational latency*: time waiting for the first bit of the target sector to pass under the r/w head.
	- *Transfer time*: time to read the bits in the target sector.
	- Access time is dominated by transfer time and rotational latency.

## Scheduling

![[Disk Scheduling Algorithms]]

# Sources
1. Extends [[Memory Hierarchy#Sources]]
2. OSTEP Chapter 37 - "Hard Disk Drives"