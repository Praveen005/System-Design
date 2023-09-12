#### What is database consistency?

> - Database consistency is defined by a set of values that all data points within the database system must align to in order to be properly read and accepted.

> - Database consistency is achieved by establishing rules. 

> - Any transaction of data written to the database must only change affected data as defined by the specific constraints, triggers, variables, cascades, etc., established by the rules set by the database’s developer.

#### What is a transaction in a database?

> - A transaction in a database is a sequence of one or more SQL operations that are treated as a single, indivisible unit of work.

#### Isolation in RDBMS

> - In a relational database management system (RDBMS), an isolation level is a concept that defines the level of visibility and interaction between concurrent transactions. Isolation levels are crucial for ensuring data consistency and preventing issues like dirty reads, non-repeatable reads, and phantom reads in multi-user database environments.

#### READ UNCOMMITTED:
> - In this isolation level, a transaction can read uncommitted changes made by other transactions. It offers the lowest level of isolation.

> - This can lead to dirty reads, non-repeatable reads, and phantom reads.

> - Yahan, uncommitted data dekh sakte, data change kar sakte, naya row insert/delete kar sakte concurrently(parallely)

#### READ COMMITTED:

> - In READ COMMITTED, a transaction can only read committed changes made by other transactions. It prevents dirty reads but allows non-repeatable reads and phantom reads.
> When a transaction uses this isolation level, a SELECT query (without a FOR UPDATE/SHARE clause) sees only data committed before the query began; it never sees either uncommitted data or changes committed during query execution by concurrent transactions. In effect, a SELECT query sees a snapshot of the database as of the instant the query begins to run. 

“Matlab, just SELECT query run karne ke time data ka snapshot lega, and us data me koi aur transaction aakar, change karta hai values,but not yet committed then, it won’t be read by transaction A, also, agar select query abhi chal hi rahi hai, and in the meanwhile, transaction B comes and commit some changes to the data, then Transaction A won’t be able to see those changes while running its SELECT query. Once this SELECT query execution is finished, then in subsequent reads or SELECT Query within the same transaction will see those changes.”

> However, SELECT does see the effects of previous updates executed within its own transaction, even though they are not yet committed. 

> Also note that two successive SELECT commands can see different data, even though they are within a single transaction, if other transactions commit changes after the first SELECT starts and before the second SELECT starts.



> - Read Committed is the default isolation level in PostgreSQL. 



#### REPEATABLE READ:

> - This isolation level ensures that a transaction sees a consistent snapshot of the data. It prevents dirty reads and non-repeatable reads but allows phantom reads.

> - The Repeatable Read isolation level only sees data committed before the transaction began; it never sees either uncommitted data or changes committed during transaction execution by concurrent transactions.

> Means, if transaction A is running and in the meantime, transaction B comes and changes a data, subsequent reads made by the transaction A under Repeatable Read Isolation level won’t see that Update/change. ye jo transaction suru hone ke pahle dekha tha, wahi yad rakhega. Mind you, DATA CHANGE CAN HAPPEN BY ANY CONCURRENT TRANSACTION, just that ‘A’ won’t look at those changes.

> Yahan, uncommitted data nahi dekh sakte, data change kar sakte(bas ye andha dekhega nahi un changes ko), naya row insert/delete kar sakte concurrently/parallely (inn changes ko dekh lega ye, pakad lega)

> - Default Isolation Level in MySQL.

> - Within the same transaction you can read the uncommitted data.

#### Serializable Isolation Level:

> - The Serializable isolation level provides the strictest transaction isolation. This level emulates serial transaction execution for all committed transactions; as if transactions had been executed one after another, serially, rather than concurrently.

> Yahan, uncommitted data nahi  dekh sakte, data change nahi  kar sakte, naya row insert/delete nahi  kar sakte concurrently(parallely)




#### The phenomena(race conditions) which are prohibited at various levels are:

> 1. Dirty Read (Isolation Level: READ UNCOMMITTED):

In this scenario, consistency is not well-maintained because Transaction A reads uncommitted data from Transaction B. Transaction A might get an incorrect or inconsistent view of the data since it's reading data that hasn't been confirmed as valid yet. Dirty reads can lead to incorrect decisions and data inconsistencies.

> 2. Non-repeatable read

Before transaction A is over(remember A isn’t over yet but in the meanwhile), another transaction B also accesses the same data and modifies it. Then, due to the modification caused by transaction B, the data when read twice by transaction A may be different.

> - Wiki says: “A non-repeatable read occurs when a transaction retrieves a row twice and that row is updated by another transaction that is committed in between.”


> 3. Phantom Read:

> It happens when suppose, I read certain rows of data in a table.
>
> Now another transaction B gets executed in the meanwhile and this transaction inserts a new row or deletes an existing row.
>
> Now in the subsequent read, of the same set of rows/data which was scanned earlier, Transaction A will find a new row inserted, this read is called Phantom Read.




**Note:**

> A lower isolation level increases the ability of many users to access data at the same time, but increases the number of concurrency effects (such as dirty reads or lost updates) users might encounter. 
> 
> Conversely, a higher isolation level reduces the types of concurrency effects that users may encounter, but requires more system resources and increases the chances that one transaction will block another. 
>
> Choosing the appropriate isolation level depends on balancing the data integrity requirements of the application against the overhead of each isolation level. 








#### Difference between "read committed" and "repeatable read" in SQL Server



> - Read committed is an isolation level that guarantees that any data read was committed at the moment is read. It simply restricts the reader from seeing any intermediate, uncommitted, 'dirty' read. It makes no promise whatsoever that if the transaction re-issues the read, will find the Same data, data is free to change after it was read.
>
>
> Repeatable read is a higher isolation level, that in addition to the guarantees of the read committed level, it also guarantees that any data read cannot change, if the transaction reads the same data again, it will find the previously read data in place, unchanged, and available to read.
>
>
> The next isolation level, serializable, makes an even stronger guarantee: in addition to everything repeatable read guarantees, it also guarantees that no new data can be seen by a subsequent read.




##### In short:
> Phantom Read: New row found/deleted in subsequent read.<br>
> Non - Repeatable Read: Data changed in subsequent read.<br>
> Dirty Read: reading the uncommitted data.



##### References: 
[PostresQL Doc](https://www.postgresql.org/docs/current/transaction-iso.html#:~:text=13.2.-,2.,transaction%20execution%20by%20concurrent%20transactions.)    
[Stackoverflow](https://stackoverflow.com/questions/4034976/difference-between-read-commited-and-repeatable-read-in-sql-server)    
[Wiki](https://en.wikipedia.org/wiki/Isolation_(database_systems))   
[Write Skew](https://stackoverflow.com/questions/48417632/why-write-skew-can-happen-in-repeatable-reads) The example explained in this StackOverflow answer has likely been taken from the Book [Designing Data-Intensive Applications](https://www.amazon.in/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/9352135245), check page 246/247.

[Write Skew 2](https://vladmihalcea.com/a-beginners-guide-to-read-and-write-skew-phenomena/)







I was reading the book, [Designing Data-Intensive Applications](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/) and noted down the following points:


>- If two transactions don’t touch the same data, they can safely be run in parallel, because neither depends on the other. Concurrency issues (race conditions) only come into play when one transaction reads data that is concurrently modified by another transaction, or when two transactions try to simultaneously modify the same data.
 
#### Read Committed
> It makes two guarantees:
> 1. When reading from the database, you will only see data that has been committed (no dirty reads).
> 2. When writing to the database, you will only overwrite data that has been committed (no dirty writes).
 
##### no dirty reads:
> Imagine a transaction has written some data to the database, but the transaction has not yet been committed or aborted. Can another transaction see that uncommitted data? If yes, that is called a dirty read.
Transactions running at the read committed isolation level must prevent dirty reads. This means that any writes by a transaction only become visible to others when that transaction commits  
 
**Note:** read uncommitted prevents dirty writes, but does not prevent dirty reads.
 
###### There are a few reasons why it’s useful to prevent dirty reads:
> If a transaction needs to update several objects, a dirty read means that another transaction may see some of the updates but not others. The user sees the new unread email but not the updated counter. This is a dirty read of the email. Seeing the database in a partially updated state is confusing to users and may cause other transactions to take incorrect decisions.
> If a transaction aborts, any writes it has made need to be rolled back. If the database allows dirty reads, that means a transaction may see data that is later rolled back i.e., which is never actually committed to the data‐ base.
 
 
##### No dirty writes:
> What happens if two transactions concurrently try to update the same object in a database? We don’t know in which order the writes will happen, but we normally assume that the later write overwrites the earlier write.
However, what happens if the earlier write is part of a transaction that has not yet been committed, so the later write overwrites an uncommitted value? This is called a dirty write. Transactions running at the read committed isolation level must prevent dirty writes, usually by delaying the second write until the first write’s transaction has committed or aborted.
> 
> **Example:** consider a used car sales website on which two people, Alice and Bob, are simultaneously trying to buy the same car. Buying a car requires two database writes: the listing on the website needs to be updated to reflect the buyer, and the sales invoice needs to be sent to the buyer.
The sale is awarded to Bob (because he performs the winning update to the listings table), but the invoice is sent to Alice (because she performs the winning update to the invoices table). Read committed prevents such mishaps.
 
 
##### Implementing read committed:
> 
> Most commonly, databases prevent dirty writes by using row-level locks: when a transaction wants to modify a particular object (row or document), it must first acquire a lock on that object. It must then hold that lock until the transaction is committed or aborted. Only one transaction can hold the lock for any given object; if another transaction wants to write to the same object, it must wait until the first transaction is committed or aborted before it can acquire the lock and continue. This locking is done automatically by databases in read committed mode (or stronger isolation levels).
> 
###### How do we prevent dirty reads? 
> One option would be to use the same lock, and to require any transaction that wants to read an object to acquire the lock briefly and then release it again immediately after reading. This would ensure that a read couldn’t happen while an object has a dirty, uncommitted value (because during that time the lock would be held by the transaction that has made the write).
> However, the approach of requiring read locks does not work well in practice, because one long-running write transaction can force many read-only transactions to wait until the long-running transaction has completed. This harms the response time of read-only transactions and is bad for operability: a slowdown in one part of an application can have a knock-on effect in a completely different part of the application, due to waiting for locks.
> For that reason, in most databases for every object that is written, the database remembers both the old committed value and the new value set by the transaction that currently holds the write lock. While the transaction is ongoing, any other transactions that read the object are simply given the old value. Only when the new value is committed do transactions switch over to reading the new value.
 
#### Non - Repeatable Read:
> Say Alice has $1,000 of savings at a bank, split across two accounts with $500 each. Now a transaction transfers $100 from one of her accounts to the other. If she is unlucky enough to look at her list of account balances in the same moment as that transaction is being processed, she may see one account balance at a time before the incoming payment has arrived (with a balance of $500), and the other account after the outgoing transfer has been made (the new balance being $400). To Alice it now appears as though she only has a total of $900 in her accounts—it seems that $100 has vanished into thin air.
> This anomaly is called a non-repeatable read or read skew: if Alice were to read the balance of account 1 again at the end of the transaction, she would see a different value ($600) than she saw in her previous query.

##### More example:
###### Backups:
> Taking a backup requires making a copy of the entire database, which may take hours on a large database. During the time that the backup process is running, writes will continue to be made to the database. Thus, you could end up with some parts of the backup containing an older version of the data, and other parts containing a newer version. If you need to restore from such a backup, the inconsistencies (such as disappearing money) become permanent.
 
> Repeatable Read isolation is the most common solution to this problem. The idea is that each transaction reads from a consistent snapshot of the database—that is, the trans‐ action sees all the data that was committed in the database at the start of the transaction. Even if the data is subsequently changed by another transaction, each transaction sees only the old data from that particular point in time.


#### lost updates:
 
> The lost update problem can occur if an application reads some value from the database, modifies it, and writes back the modified value (a read-modify-write cycle). If two transactions do this concurrently, one of the modifications can be lost, because the second write does not include the first modification.
Many databases provide atomic update operations, which remove the need to implement read-modify-write cycles in application code.
> Another option for preventing lost updates, if the database’s built-in atomic operations don’t provide the necessary functionality, is for the application to explicitly lock objects that are going to be updated.

#### Write Skew:

> It is also a race condition, caused due to updation of two different objects.
check page 246,247 in book: [Designing Data-Intensive Applications](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/)




<details>
  
<summary>Caution: <sub><sup>(click to expand)</sup></sub></summary>

There's a lot to it, this was just a brief overview.  
I know, This is not structured well, will improve going forward.  
Feel free to add or point out inaccuracies(if any).

</details>
