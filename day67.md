Transaction
- It is a logical unit of work done in a db that contains one or more SQL statements. The result of all these statements in a transaction either gets completed successfully (all the changes made to the database are permanent) or if at any point any failure happens it gets rollbacked (all the changes being done are undone.)
- Consists of multiple CRUD operations

ACID Properties
- To ensure integrity of the data, we require that the DB system maintain ACID properties of the transaction.
- Atomicity - Either all operations of transaction are reflected properly in the DB, or none are. Pass or fail basically. eg- Money transfer either done or not done due to system failure. That's why a db maintains both old state and intermediate state. In case of transaction failure db rolls back to old state. eg-> Suppose A is sending money to B. The amount of money in A's account is read and then 50 rupees is deducted from it. But then the system fails. In this case, the db rollsback to the old state.
- Consistency - DB must be consistent before and after the transaction happens. eg- before transaction if account A+B has x amount of money then after transaction also A+B should have x amount.
- Isolation - Multiple transactions can happen in the system in isolation, without interfering each other. I.e. the process on the second state of the database will start after the operation on the first state is finished. eg. If A is paying B through both UPI and netbanking at the same time, and the current amount in the account of A is read at the same time by both systems then both the UPI and netbanking will think that A has 1000 rupees in its account. Now the transaction of 500 rupees is done by both UPI and netbanking to B then B will receive 1000 rupees but only 500 rupee is deducted from A's account. That's why we need isolation in transactions to maintain its integrity.
- Durability - After transaction completes successfully, the changes it has made to the database persist, even if there are system failures. eg- If user has been given the response that 500rs were debited from his account to some other user then the credit and debit should actually happen even if the system failed. So durability is basically the ability of DB to recover. It is done by making logs of the transaction so that when the DB recovers from system failure the transaction can be completed by looking at the logs.