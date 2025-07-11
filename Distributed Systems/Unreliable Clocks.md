# Explanation
- Each machine on the network has its own clock, which is an actual hardware device: usually a quartz crystal oscillator.
	- These devices are not perfectly accurate, so each machine has its own notion of time, which may be slightly faster or slower than on other machines.
- It is possible to synchronize clocks to some degree: the most commonly used mechanism is the Network Time Protocol ([[NTP]]), which allows the computer clock to be adjusted according to the time reported by a group of servers, which in turn get their time from a more accurate time source, such as a GPS receiver.
- Our methods for getting a clock to tell the correct time aren’t nearly as reliable or accurate as you might hope—hardware clocks and NTP can be fickle beasts.

## Types of Clocks

### Time-of-day clocks
- A time-of-day clock returns the current date and time according to some calendar (also known as wall-clock time).
- Time-of-day clocks are usually synchronized with NTP.
- if the local clock is too far ahead of the NTP server, it may be forcibly reset and appear to jump back to a previous point in time.
- These jumps, as well as similar jumps caused by leap seconds, make time-of-day clocks unsuitable for measuring elapsed time.

### Monotonic clocks
- A monotonic clock is suitable for measuring a duration (time interval), such as a timeout or a service’s response time.
- They are guaranteed to always move forward (whereas a [[#Time-of-day clocks|time-of-day clock]] may jump back in time).
- You can check the value of the monotonic clock at one point in time, do something, and then check the clock again at a later time.
	- The difference between the two values tells you how much time elapsed between the two checks.
- The absolute value of the clock is meaningless.
- NTP may adjust the frequency at which the monotonic clock moves forward if it detects that the computer’s local quartz is moving faster or slower than the NTP server.
	- This is known as *slewing* the clock.
- By default, NTP allows the clock rate to be speeded up or slowed down by up to 0.05%, but NTP cannot cause the monotonic clock to jump forward or backward.

### Logical Clocks
- **Logical clocks** are based on incrementing counters rather than an oscillating quartz crystal, are a safer alternative for ordering events.
- Logical clocks do not measure the time of day or the number of seconds elapsed, only the relative ordering of events.
- In contrast, time-of-day and monotonic clocks, which measure actual elapsed time, are also known as physical clocks.

## Relying on Synchronized Clocks

### Timestamps for ordering events
- This conflict resolution strategy is called last write wins (LWW).
- It has fundamental problems:
	- **Database writes can mysteriously disappear**: a node with a lagging clock is unable to overwrite values previously written by a node with a fast clock until the clock skew between the nodes has elapsed.
		- This scenario can cause arbitrary amounts of data to be silently dropped without any error being reported to the application.
	- **LWW cannot distinguish between writes that occurred sequentially in quick succession  and writes that were truly concurrent** (neither writer was aware of the other).
	- **It is possible for two nodes to independently generate writes with the same timestamp**, especially when the clock only has millisecond resolution.
	- An additional tiebreaker value (which can simply be a large random number) is required to resolve such conflicts, but this approach can also lead to violations of causality.

### Confidence Intervals
- It doesn’t make sense to think of a clock reading as a point in time—it is more like a range of times, within a confidence interval.
- The uncertainty bound can be calculated based on the time source.
	- If you’re getting the time from a server, the uncertainty is based on the expected quartz drift since your last sync with the server, plus the NTP server’s uncertainty, plus the network round-trip time to the server.
- Unfortunately, most systems don’t expose this uncertainty: for example, when you call `clock_gettime()`, the return value doesn’t tell you the expected error of the timestamp.

### Synchronized clocks for global snapshots
- The most common implementation of [[Transaction Isolation Levels#Snapshot Isolation|snapshot isolation]] requires a monotonically increasing transaction ID.
- If a write happened later than the snapshot (i.e., the write has a greater transaction ID than the snapshot), that write is invisible to the snapshot transaction.
- On a single-node database, a simple counter is sufficient for generating transaction IDs.

- When a database is [[Distributed Systems|distributed]] across many machines, potentially in multiple datacenters, a global, monotonically increasing transaction ID (across all partitions) is difficult to generate, because it requires coordination.
- The transaction ID must reflect causality: if transaction B reads a value that was written by transaction A, then B must have a higher transaction ID than A—otherwise, the snapshot would not be consistent.
- With lots of small, rapid transactions, creating transaction IDs in a distributed system becomes an untenable bottleneck.

# Sources 
- Designing Data-Intensive Applications - Chapter 8
