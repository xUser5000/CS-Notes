# Explanation
- A hard disk drive (HDD) is an electro-mechanical data storage device that stores and retrieves digital data using magnetic storage with one or more rigid rapidly rotating platters coated with magnetic material.
- Internal structure:
	- Disks consist of platters, each with two surfaces.
	- Each surface consists of concentric rings called tracks.
	- Each track consists of sectors separated by gaps.
	- Aligned tracks form a cylinder.
	- Modern disks partition tracks into disjoint subsets called recording zones.
		- Each track in a zone has the same number of sectors.
		- Each zone has a different number of sectors.
		- We use the average number of tracks when calculating the disk capacity.
	- Illustration: ![[What's inside an HDD.png]]
- Computing the disk capacity: ![[Disk capacity formula.png]]
- The access time of some target sector = seek time + rotational latency + transfer time.
	- Seek time: time to position the head under the target sector (3 - 9 ms).
	- Rotational latency: time waiting for the first bit of the target sector to pass under the r/w head.
	- Transfer time: time to read the bits in the target sector.
	- Access time is dominated by transfer time and rotational latency.

# Sources
- Extends [[Memory Hierarchy#Sources]]