# Test iso;oation locks in different DB's
By changing isolation levels and making parallel queries, reproduce the main problems of parallel access: lost update, dirty read, non-repeatable read, phantom read


# PSQL TEST
### Lost update
A lost update occurs when two transactions read the same data, perform some operation based on that data, and then one transaction updates the data before the other transaction, causing the first transaction's update to be lost.
<img width="1440" alt="Screenshot 2024-04-10 at 15 26 51" src="https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/c05d5dae-874b-4a98-b346-61895880da23">

### Dirty read
A dirty read occurs when one transaction reads data that has been modified by another transaction but not yet committed. If the modifying transaction rolls back, the data read by the first transaction becomes invalid.

To make this work we should also set the isolation level read uncommited.

Tried to reproduce, have some problems. 
![Screenshot 2024-04-10 at 15 37 56](https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/b02f08b0-e84a-4cd6-ba0b-0253e6f0b6c1)
![Screenshot 2024-04-10 at 15 37 08](https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/248b4b8f-952f-4a82-9114-e9f044e97356)


### Non-repeatable Read
A non-repeatable read occurs when a transaction reads the same data multiple times within a single transaction but gets different results due to updates by other transactions in between.

### Phantom Read
A phantom read occurs when a transaction reads a set of records that satisfy some search condition, but while it is reading the records, another transaction inserts or deletes records that also satisfy the search condition, causing the first transaction to see additional records or miss some records when it reads again.
