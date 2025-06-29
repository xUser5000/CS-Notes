# Explanation
- Extends [[Database Storage#Tuple Layout]]
- [[Database Storage#Database Heap|Heap]] tuples in table pages are classified into two types: usual data tuples and TOAST tuples.
- A heap tuple comprises three parts: the `HeapTupleHeaderData` structure, NULL bitmap, and user data.
	- Illustration: ![[PostgreSQL Tuple Structure.png]]
- The `HeapTupleHeaderData` structure contains seven fields, but only four of them are focused:
	- `t_xmin` holds the txid of the [[PostgreSQL Transactions|transaction]] that inserted this tuple.
	- `t_xmax` holds the txid of the transaction that deleted or updated this tuple.
		- If this tuple has not been deleted or updated, `t_xmax` is set to 0, which means INVALID.
	- `t_cid` holds the command id (cid), which is the number of SQL commands that were executed before this command was executed within the current transaction, starting from 0.
	- `t_ctid` holds the tuple identifier (tid) that points to itself or a new tuple.
		- When this tuple is updated, the `t_ctid` of this tuple points to the new tuple; otherwise, the `t_ctid` points to itself.

# Sources
- [InterDB - 5. Concurrency Control](https://www.interdb.jp/pg/pgsql05.html)
