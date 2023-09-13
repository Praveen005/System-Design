> - Relational Databases are the most critical components of any system. They can make or Break a system.

> - If it goes down, it will take everybody with it.

> - In Relational Databases, data are stored in Rows or Columns.

> - A Database should have the following properties:
> > - Data Consistency<br>
> > - Data Durability<br>
> > - Data Integrity<br>
> > - Constraints<br>
> > - Everything in one place

> Most people use Relational Databases when they need transaction.
> Transactions are the atomic unit of database.

> - The reason relational databases are famous because they give **ACID** guarantees.
> > A: Atomicity<br>
> > C: Consistency<br>
> > I: Isolation<br>
> > D: Durability

##### Atomicity: 
> Either all the statements within a transactions take effect or none of them take effect.
>
> **Example 1:**
> I any financial system, If I were to insert two different tables, we can wrap it in a transaction,<br>
> such that either both of them happens or none.<br>
> It will never happen that data gets stored in one table and the system crashed before data getting logged into<br>
> the second table, and now we are having an inconsistent state.

> - Atomicity Ensures:
> No matter how many statements I run in a transaction, either all of them would be executed or none of them would<br>
> be executed.

> **Example 2:**
> Say, we have two actions to perform-<br>
> > Publish a post
> > Increase the count of posts( post_count = post_count + 1).
> It shouldn't happen that, we published a post but the count of posts<br>
> hasn't increased. It should be consistent. That's why we wrap both "Insert" and "Update" statements in a single transaction.

> **Example 3:**
> Suppose you transferred money from account 'A' to 'B'. Money got debited from 'A' but system crashed before being credited to 'B'.
> What would happen?
> Money vanished in thin air? can't say this na.
> This is why we need Atomicity.


##### Consistency:
> - Data will never go incorrect, no matter what.

> **Example 1:** 
> If I have just $100 in bank account, I shouldn't be able to transfer $150.
> Consistency is achieved via Constraints, Cascades, Triggers etc.
> > Constraints: 
> > > We can define it on Relational Databases:
> > > > Check Constraints
> > > > Foreign Key Constraints, etc.
> > > > > Example: Foreign key checks don't allow you to delete parent, if child exists
> > Cascades:
> > > It says, if you delete aparticular row, you want to delete everything associated with it.
> > > > Example: You delete the parent table, the children table gets deleted.
> > > > > Suppose there is a users table and another is posts table, if I delete a User entry, then all his posts should also get deleted. This is what CASCADES ensure.
> > Triggers:
> > > Example: We can use trigger statement to do something like, say in that facebok post, we can use trigger:
> > > When a user publishes a post, trigger an SQL statement to update the total_posts column. 
> > > Google for more :p


> > Durability:
> > > If a transaction is committed, no matter system crashes or whatever, as long as your hard drive is there, your data should be safe.
> > > **When a transaction commits, your changes should outlive the outage.**
> > > The data just can't vanish out of the blue.

> > > The Relational Database gives you the guarantee, that when you commit any transaction, that commit is being written on the disk and it will be there no matter what.
