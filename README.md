# Fundamentals-of-Database-Engineering

This repo will contain notes for the course: [Fundamentals of Database Engineering](https://www.udemy.com/course/database-engines-crash-course/learn/lecture/28927754#overview).<br/>

## ACID

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
