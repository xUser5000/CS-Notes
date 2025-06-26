# Explanation
- DBMS use [[Thread]]s to:
	- take advantage of additional CPU cores.
	- hide [[Disks]] IO stalls.
- *Concurrency Control Protocol*: the method used by the DBMS to ensure correct results for concurrency operations on a shared object.

## Locks vs Latches

|                          | **Lock**                                                                                                                  | **Latch**                                                                                                          |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Definition**           | higher-level primitive that protects the contents of a database (e.g, tuples, tables, databases) from other transactions. | low-level protection primitive used for critical sections in the DBMS internal data structures from other threads. |
| **Held for**             | the duration of the transaction.                                                                                          | the operation being made.                                                                                          |
| **Exposed to the user?** | Yes                                                                                                                       | No                                                                                                                 |
| **Supports rollback?**   | Yes                                                                                                                       | No                                                                                                                 |

## Latch Implementation

#### Blocking OS Mutex
- **Description**: see [[Lock#^565747]]
- **Example**: `std::mutex`
- **Advantage**: Simple to use and requires no additional coding in the DBMS.
- **Disadvantage**: Expensive and non-scalable because of OS scheduling.

#### Test-and-Set Spln Latch
- **Description**:
	- Extends [[Spin Lock with Yield]].
	- The DBMS can control what happens if it fails to acquire the latch.
- **Example**: `std::atomic<T>`
- **Advantage**: Latch/Unlatch operations are efficient.
- **Disadvantages**: 
	- Not scalable.
	- Not cache-friendly.

#### Reader-Writer Latch
- **Description**:
	- allows the larch to be held in either reader or writer mode.
	- keeps track of the number of readers threads threads the hold the latch and are waiting to acquire it in each mode.
	- uses either [[#Blocking OS Mutex]] or [[#Test-and-Set Spln Latch]] as the underlying implementation and has additional logic to handle reader-writer queues.
- **Example**: `std::shared_mutex<T>`
- **Advantage**: Allows for concurrent readers.
- **Disadvantages**:
	- The DBMS has to manage read/write queues to avoid starvation.
	- Has larger storage overhead than spin latches due to additional metadata.

# Sources
- CMU 15-445 Lecture 9 - "Index Concurrency Control"