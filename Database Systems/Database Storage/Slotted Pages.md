# Explanation
- *Slotted Pages*: contains a slot array that maps the slots to the tuple's starting positions.
- Header keeps track of the number of used slots, the offset of the starting location of the last used slot, and a slot array, which keeps track of the location of the start of each tuple.
- To add a tuple, the slot array will grow from the beginning to the end, and the data of the tuples will grow from end to the beginning. The page is considered full when the slot array and the tuple data meet.
- Most common approach used in DBMSs today.

## Advantages
- Fast reads.
- No need for garbage collection.

## Disadvantages
- Fragmentation: Deletion of tuples can leave gaps in the pages.
- Useless Disk I/O: Due to the block-oriented nature of non-volatile storage, the whole block needs to be read to fetch a tuple.
- Random Disk I/O: The disk reader could have to jump to 20 different places to update 20 different tuples, which can be very slow.

# Sources
- Extends [[Database Storage#Sources]]
- 