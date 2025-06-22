# Explanation
- Extends [[Concurrency Control]]
- PostgreSQL and some [[Database Systems|RDBMSs]] use a variation of [[Multi-versioning Concurrency Control (MVCC)|MVCC]] called Snapshot Isolation (SI).
- SI does not allow the three anomalies defined in the ANSI SQL-92 standard: [[Transaction Anomalies#Dirty Reads|Dirty Reads]], [[Transaction Anomalies#Read Skew (non-repeatable reads) [1]|Non-Repeatable Reads]], and [[Transaction Anomalies#Phantom Reads [1]|Phantom Reads]]. However, SI cannot achieve true [[Concurrency Control Protocols#^ec3fa1|serializability]] because it allows serialization anomalies, such as [[Transaction Anomalies#Write Skew [1]|Write Skew]] and _Read-only Transaction Skew_.
- [[Serializable Snapshot Isolation (SSI)]] has been added as of version 9.1. SSI, which allowed PostgreSQL to provide a true SERIALIZABLE isolation level.

## Mechanism
- A new data item is inserted directly into the relevant table page.
- When reading items, PostgreSQL selects the appropriate version of an item in response to an individual transaction by applying **visibility check rules**.

## Topics
- [[PostgreSQL Transactions]]

# Sources
- [InterDB - 5. Concurrency Control](https://www.interdb.jp/pg/pgsql05.html)
