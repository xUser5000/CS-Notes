# Explanation
- Illustration of System architecture with respect to IO devices: ![[System Architecture with respect to IO.png]]

## IO Device Structure
![[IO Device Structure.png]]
- **Hardware Interface**: allows the system software to control its operation.
- **Internal Structure**: responsible for implementing the abstraction presented to the system.
	- device-specific

## IO Device Protocols

### Pre-programmed IO
- **Description**: The CPU is involved in data movement.
- **Mechanism**: ![[Pre-programmed IO Protocol.png]]
- **Advantages**: simple and working.
- **Disadvantages**: polling wastes a great deal of CPU time.

### Interrupt-based IO
- **Description**: 
- **Mechanism**:
	- The OS issues an IO request, put the calling thread to sleep, and does a #context_switch to another process.
	- When the device is finished with IO, it raises a hardware interrupt and the OS handle it via #interrupt_handler.
- **Advantages**:
	- Extends [[#Pre-programmed IO]] advantages
	- Faster since it allows for CPU overlap.
- **Disadvantages**: A Huge number of interrupts can cause the OS to #livelock (only process interrupts).

### Direct Memory Access (DMA)
- **Description**: 
	- #DMA_engine:a device within a system that orchestrates transfers between devices and main memory without much CPU intervention.
- **Mechanism**:
	- The OS programs the #DMA_engine by telling where the data lives in memory, how much data to copy, and which device to send it to.
	- When the #DMA_engine is complete, the controller raises and interrupt so that the OS knows the transfer is complete.
- **Advantages**:
	- Extends [[#Interrupt-based IO]] advantages
	- Does not require any intervention from the CPU.
- **Disadvantages**:

## IO Device Interaction
- The OS can interact with IO devices using the following methods:
	- *Explicit IO Instructions*: sends data to specific device registers to implement [[#IO Device Protocols]].
		- x86 instruction set has `in` and `out`
		- privileged instruction
	- *Memory-mapped IO*:
		- hardware makes device registers available as if they were memory locations.
		- To access a particular register, the OS issues a `load`/`store` instruction and the hardware routes these instructions to the device.
- Both approaches are still in use today.

### IO Device Driver
- It's a piece of software in the OS that knows in detail how a device works.
	- Any specifics of device interaction are encapsulated in it.
- They have come to represent a huge percent of kernel code.

# FAQ
- Why do we need a hierarchical structure to organize system IO devices?
	- The faster a bus is, the shorter is must be.
		- Components that demand high performance are near the CPU and vice versa.
	- Engineering a high performance bus is costly.

# Sources
- OSTEP Chapter 36 - "I/O Devices"