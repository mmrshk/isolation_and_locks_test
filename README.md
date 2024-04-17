# Test isolation locks in different DB's
By changing isolation levels and making parallel queries, reproduce the main problems of parallel access: lost update, dirty read, non-repeatable read, phantom read


### Lost update
A lost update occurs when two transactions read the same data, perform some operation based on that data, and then one transaction updates the data before the other transaction, causing the first transaction's update to be lost.

#### PSQL
<img width="1440" alt="Screenshot 2024-04-10 at 15 26 51" src="https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/c05d5dae-874b-4a98-b346-61895880da23">

#### MySQL
<img width="1440" alt="Screenshot 2024-04-17 at 21 04 16" src="https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/231d063d-a19d-4f1f-a3d6-73d970fd4ab0">


### Dirty read
A dirty read occurs when one transaction reads data that has been modified by another transaction but not yet committed. If the modifying transaction rolls back, the data read by the first transaction becomes invalid.

#### PSQL
To make this work we should also set the isolation level read uncommited.
Tried to reproduce, and I had problems. 

<img width="721" alt="Screenshot 2024-04-10 at 15 42 30" src="https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/8cd78bac-672d-4e7a-846b-bcce596af413">

#### Result
In PostgreSQL, the READ UNCOMMITTED isolation level is similar to READ COMMITTED, where transactions only see data that has been committed. 
This means that behavior for READ UNCOMMITTED isolation level differs from other DB's.
It's not feasible to reproduce dirty read behavior in PSQL. 

#### MySQL
<img width="1417" alt="Screenshot 2024-04-17 at 21 13 33" src="https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/f0300853-d1ff-46e3-8003-5e7a7094a52e">

### Non-repeatable Read
A non-repeatable read occurs when a transaction reads the same data multiple times within a single transaction but gets different results due to updates by other transactions in between.

#### PSQL
<img width="1438" alt="Screenshot 2024-04-11 at 09 11 02" src="https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/2257bf0a-b483-448e-9d41-99ebf7235e7b">

#### MySQL
<img width="1440" alt="Screenshot 2024-04-17 at 21 20 02" src="https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/e54c20ab-c39b-4b25-a430-7fa4392d0fac">


### Phantom Read
A phantom read occurs when a transaction reads a set of records that satisfy some search condition, but while it is reading the records, another transaction inserts or deletes records that also satisfy the search condition, causing the first transaction to see additional records or miss some records when it reads again.

#### PSQL
<img width="1440" alt="Screenshot 2024-04-11 at 09 19 21" src="https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/ec6f2907-0028-4b24-a147-9bc9fcabfe26">

#### MySQL
<img width="1440" alt="Screenshot 2024-04-17 at 21 23 08" src="https://github.com/mmrshk/isolation_and_locks_test/assets/31416671/b81be5ba-69a0-4863-b391-371f2d1b7be3">

