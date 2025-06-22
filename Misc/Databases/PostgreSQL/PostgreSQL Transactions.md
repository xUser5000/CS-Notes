# Explanation
- Extends [[Concurrency Control]]
- At the start of every transaction, the transaction manager assigns a unique identifier known as a transaction ID (txid).
- PostgreSQL’s txid is a 32-bit unsigned integer, with a maximum value of approximately 4.2 billion.
- PostgreSQL reserves the following three special txids:
	- **0** means **Invalid** txid.
	- **1** means **Bootstrap** txid, which is only used in the initialization of the database cluster.
	- **2** means **Frozen** txid.
- Txids can be compared with each other. For example, at the viewpoint of txid 100, txids that are greater than 100 are ‘_in the future_’ and are _invisible_ from the txid 100; txids that are less than 100 are ‘_in the past_’ and are _visible_.
- Since the txid space is insufficient in practical systems, PostgreSQL treats the txid space as a circle. The previous 2.1 billion txids are ‘in the past’, and the next 2.1 billion txids are ‘in the future’.
	- Illustration: ![[PostgreSQL Transaction ID space.png]]


- Note that BEGIN command does not be assigned a txid.
- In PostgreSQL, when the first command is executed after a BEGIN command executed, a tixd is assigned by the transaction manager, and then the transaction starts.

# Sources
- [InterDB - 5. Concurrency Control](https://www.interdb.jp/pg/pgsql05.html)
