A transaction is the DBMS’s abstract view of a user program: a series of reads/writes of database objects. 
A transaction is seen by the DBMS as a series, or list, of actions. It should finalize its action with either a *commit* or an *abort*. 
The concurrency is achieved by the DBMS, which interleaves actions of the various transactions.

### The ACID Properties

#### Atomicity
Either all actions of a transactions are carried out or none of them (0 or 100%).
A transaction interrupted in between actions can lead to inconsistency in database. DBMS needs to remove the effects of partial transactions to ensure atomicity.

**How does DBMS ensures recovery happens?**
- Undoes the effects of partial transactions
- It keeps a *log* of all the actions that it does during a transaction.
- *Recovery Manager* is responsible for making sure mistakes are taken back.

#### Consistency
When a transaction is run to completion on a consistent database, it should leave the database in a consistent state. 
*Database consistency* is the property that every transaction sees a consistent database instance. It
follows from transaction atomicity, isolation and transaction consistency.


#### Isolation
Intermediate transaction results must be hidden from other concurrently executed transactions

Guarantee that even though transactions may be interleaved, the net effect is identical to executing the transactions serially.
*Though it should be noted that DBMS does not guarantee about the order of execution of transactions.*

#### Durability
If a transaction completes successfully, then its effects persist (the changes it has made to the database should be permanent)

DBMS uses the log to ensure durability.

### Scheduling actions
A schedule is a list of actions from a set of transactions. 
A well-formed schedule is one where the actions of a particular transaction T are in the same order as they appear in T
![[Pasted image 20240831145428.png]]

T1: [R(a), W(a), R(c), W(c)]
T2: [R(b), W(b)]

- *Complete Schedule:* one that contains an abort or commit action for every transaction that occurs in the schedule
- *Serial Schedule:* one where the actions of different transactions are not interleaved.

### Anomalies with interleaved execution
Two actions on the same data object conflict if at least one of them is a write.

#### WR Conflicts
Transaction T2 tries reading data that T1 has changed but not yet committed. Also called *Dirty Read*
![[Pasted image 20240831150129.png]]

#### RW Conflicts
Transaction T2 could change the value of anobject that has been read by a transaction T1, while T1 is still in progress. Also called *Unreadable Reads*
![[Pasted image 20240831152418.png]]

#### WW Conflicts 
Transaction T2 could overwrite the value of an object which has already been modified by T1, while T1 is still in progress. Also called *Blind Write*
![[Pasted image 20240831152721.png]]

#### 2 Phase Locking
Each transaction must obtain *Shared (S)* lock before reading and *Exclusive (X)* lock before writing to avoid conflicts
All locks held by a transaction are released when the transaction completes
If a transaction holds an X lock on an object, no other transaction can get a lock (S or X) on that object.

### Transaction Mgmt. in NoSQL

#### BASE Transactions
*B*asically *A*vailable, *S*oft state, *E*ventually Consistent.
- Weak consistency – stale data OK
- Availability first
- Best effort delivery of data
- Approximate answers OK
- Aggressive (optimistic)
- Simpler and faster

#### CAP Theorem 
![[Pasted image 20240831153534.png]]

**Consistency:** Each client has the same view of the data
**Availability:** Each client can always read and write
**Partition Tolerance:** System works well across distributed physical networks

>At most 2/3 can be maximized at one time

