# Explanation
- Prerequisite: [[PostgreSQL Tuple Structure]]

## Insertion
- Suppose that a tuple is inserted in a page by a transaction whose txid is $X$. In this case, the header fields of the inserted tuple are set as follows:
- Tuple_1:
    - `t_xmin` is set to $X$ because this tuple is inserted by txid $X$.
    - `t_xmax` is set to $0$ because this tuple has not been deleted or updated.
    - `t_cid` is set to $0$ because this tuple is the first tuple inserted by txid $X$.
    - `t_ctid` is set to $(0,1)$, which points to itself, because this is the latest tuple.
- Illustration: ![[PostgreSQL Insertion Operation.png]]

## Deletion
- In the deletion operation, the target tuple is deleted logically; The value of the txid that executes the DELETE command is set to the `t_xmax` of the tuple.

- If a transaction with txid $X$ deletes a tuple, its `t_xmax` is set to $X$.
- If txid $X$ is committed, the is no longer required (AKA **dead tuple**).
- Dead tuples should eventually be removed from pages via **VACUUM** processing,

- Illustration: ![[PostgreSQL Deletion Operation.png]]

## Update
- In the update operation, PostgreSQL logically deletes the latest tuple and inserts a new one.

### Mechanism
- Suppose that the row, which has been inserted by txid 99, is updated twice by txid 100.
- When the first UPDATE command is executed, `Tuple_1` is logically deleted by setting txid 100 to the `t_xmax`, and then `Tuple_2` is inserted.
- Then, the `t_ctid` of `Tuple_1` is rewritten to point to `Tuple_2`.

### Illustration
![[PostgreSQL Update Operation.png]]

## Free Space Map
- When inserting a heap or an index tuple, PostgreSQL uses the **Free Space Map (FSM)** of the corresponding table or index to select the page which can be inserted into.
- All tables and indexes have respective FSMs.
- Each FSM stores the information about the free space capacity of each page within the corresponding table or index file.
- All FSMs are stored with the suffix ‘fsm’, and they are loaded into [[PostgreSQL Memory Architecture#Shared Memory Area|shared memory]] if necessary.

# Sources
- [InterDB - 5. Concurrency Control](https://www.interdb.jp/pg/pgsql05.html)
