# Fundamentals-of-Database-Engineering

This repo will contain notes for the course: [Fundamentals of Database Engineering](https://www.udemy.com/course/database-engines-crash-course/learn/lecture/28927754#overview).<br/>

## ACID

These are the set of properties of database transactions that guarantee data integrity despite errors, system failures, power failures, or other issues

### Transactions

A transaction is essentialy a bunch of queries grouped and treated as a single unit of work

Transaction Lifecycle:

- BEGIN: As the name suggests, we are signifying the start of a transaction
- COMMIT: This signifies the end of a transaction and will permanently write and save all changes made from the transactions to the db/disk
- ROLLBACK: This is used to revert all the changes from our current transactions so that our database goes back to the state of the previous commit
- Unexpected Ending (crash): If the database crashes during our transaction we need to rollback to the previous commit
- Different DB's do this differently and optimise different things

Nature of Transactions:

- Usually transactions are used to change and modify data
- Transactions can however be used to just read data (read only transactions)
- E.g. You want to generate a report and you want to get consistent snapshot data at the time of the transaction

### Atomicity

- The principle of atomicity is that all the queries of a transaction must succeed, otherwise we must rollback.
- In the case of a crash we must also always rollback, to ensure the integrity of our data.
- In both cases any succesful queries that happened prior to the crash must be reverted

## Isolation

- When we have multiple transactions being issued concurrently to our database
- e.g. Our DB supports multiple TCP connections (remote connection)
- We can have the issue of multiple transactions trying to access or modify the same data
- This can cause huge problems and inconsistencies if not handled properly
- Ultimately the question is, "Can an in flight transaction see or modify changes made by other transactions that are also in flight?"
- i.e. Do we want changes to be seen across transactions?

- Side effects from these concurrent transactions are called **Read Phenomenas**
- There are different kinds of read phenomena that can occur, each being difficult to debug
- Isolation levels were introduced in order to solve these read phenomenas
- Likewise, we also have different types of isolation levels

Read Phenomena:

- Dirty Reads: These are reads we get from another in flight transaction which has not yet been comitted, meaning that there is a chance for said transaction to crash or rollover
- Non-repeatable reads: This is when we read a value multiple times within a transaction (with different queries), wherein subsequent reads, the value will have changed, due to other concurrent transactions which have been committed. They key difference is that this read phenomena is caused by committed transactions.
- Phantom reads: This is when during our transaction we read a value multiple times and in subsequent attempts, we get new values (another value was comitted from another transaction). This new row is regarded as a "phantom row"
- Lost Updates: This occurs when multiple transactions begin at the same time, and we do different UPDATE operations on a record within these transactions, when the latter transaction is committed, our previous updates will be overwritten or "lost"

Isolation Levels:

Isolation levels were introduced in order to combat and fix the read phenomena above.
When Starting a transaction we set the isolation level using `SET ISOLATION LEVEL`, and consist of the following:

- Read Uncommitted: With this level there is actually no isolation, and any change that occurs concurrently on the outside will affect our current transaction, committed or not.
- Read Committed: As the name suggests, this isolation level will allow us to read only comitted transactions. So if we have a long running transaction and the value of a record we previously read changes, we will receive that changed value
- Repeatable Read: This isolation level ensures that whenever a query reads a row, that record/row will remain unchanged for the rest of the transactions. Phantom reads can still sneak in with this isolation level.
- Snapshot: As the name suggests, this isolation level provides us of a snapshot of the database from the moment of the transaction. So each query will only see changes that have been committed up to the start of the transaction. It eliminates all read phenomenas
- Serializable: As the name suggests with this level of isolation, there is no more concurrency and each transaction is ran one after the other, making it very slow

## Consistency

When talking about consistency in database we focus on 2 things:

- Consistency in Data: Is what he have in disk consistent with the data model we have 
    - This is defined by the user (who designs the database model)
    - Referential Integrity: Foreign Key values should be consistent and equal across different tables
    - Atomicity: Ensuring that only completed transactions are comitted
    - Isolation: Ensuring that conficting transations don't cause inconsistent data 

- Consistency in Reads: Reads of data becomes inconsistent as we have multiple instances of snapshots, which become out of sync  
    - If a transaction commits a change, will a new transaction immediately see that change?   
    - Replicas and Primary which read and write to each other can be inconsistent
    - Eventual Consistency: With replicas, as soon as all changes been synced, everything will become consistent again 