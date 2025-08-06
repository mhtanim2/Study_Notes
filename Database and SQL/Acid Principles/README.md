# 1. ACID Principles

## 1.1. Transaction

### What
- A collection of *queries* that work as a *unit* of work.

### Why
- A single task may need multiple queries to accomplish a transaction.
- Helps in handling inconsistencies.

### How

#### Commit Example

```sql
-- Start a new transaction
BEGIN TRANSACTION;

-- Step 1: Debit Account A
UPDATE Accounts
SET Balance = Balance - 100
WHERE AccountID = 1;

-- Step 2: Credit Account B
UPDATE Accounts
SET Balance = Balance + 100
WHERE AccountID = 2;

-- Check for potential issues (e.g., insufficient funds in Account A)
-- IF (SELECT Balance FROM Accounts WHERE AccountID = 1) < 0 THEN
--    ROLLBACK TRANSACTION; -- Undo all changes if balance becomes negative
-- ELSE
--    COMMIT TRANSACTION; -- Finalize the changes if no issues
-- END IF;

-- If all operations are successful, commit the transaction
COMMIT TRANSACTION;
```

#### Rollback Example

```sql
BEGIN TRANSACTION;

-- Step 1: Debit Account A
UPDATE Accounts
SET Balance = Balance - 100
WHERE AccountID = 1;

-- Simulate an error or condition that requires a rollback
-- For instance, if Account B does not exist, or if a constraint is violated.
-- In a real application, this would be handled by error checking or exception handling.
-- For demonstration, let's assume an error occurs here.

-- If an error or an unexpected condition arises, undo all changes
ROLLBACK TRANSACTION;
```

### Account Balances

| ACCOUNT_ID | BALANCE (Before) | BALANCE (After) |
|------------|------------------|-----------------|
| 1          | 1000             | 900             |
| 2          | 500              | 600             |

![Transaction Diagram](./resource/Transaction.png)

---

# 2. ACID Principles Explained

ACID stands for Atomicity, Consistency, Isolation, and Durability. These are the four key properties that ensure reliable processing of database transactions.

---

## 2.1. Atomicity

### What
- Atomicity ensures that a transaction is treated as a single, indivisible unit. Either all operations succeed, or none do.

### Why
- Prevents partial updates to the database, which could lead to data corruption or inconsistency.
- After we restarted the machine the first account has been debited but the other account has not been credited.
- This is really bad as we just lost data, and the information is inconsistent
- An atomic transaction is a transaction that will rollback all queries if one or more queries failed.
- The database should clean this up after restart.

### How

```sql
BEGIN TRANSACTION;

UPDATE Accounts SET Balance = Balance - 100 WHERE AccountID = 1;
UPDATE Accounts SET Balance = Balance + 100 WHERE AccountID = 2;

-- If any update fails, rollback the entire transaction
ROLLBACK TRANSACTION;

-- If all succeed, commit the transaction
COMMIT TRANSACTION;
```

| Step         | Account 1 | Account 2 | Status         |
|--------------|-----------|-----------|---------------|
| Before Txn   | 1000      | 500       | Initial State |
| After Debit  | 900       | 500       | In Progress   |
| After Credit | 900       | 600       | In Progress   |
| On Failure   | 1000      | 500       | Rolled Back   |
| On Success   | 900       | 600       | Committed     |

---

## 2.2. Consistency

### What
- Consistency ensures that a transaction brings the database from one valid state to another, maintaining all predefined rules and constraints.

### Why
- Guarantees that only valid data is written to the database, preserving data integrity.

### How

```sql
-- Assume a constraint: Balance >= 0

BEGIN TRANSACTION;

UPDATE Accounts SET Balance = Balance - 1200 WHERE AccountID = 1; -- Would violate constraint

-- This will fail if Balance goes below 0, and the transaction will be rolled back
COMMIT TRANSACTION;
```

| AccountID | Balance Before | Amount Debited | Balance After | Constraint Satisfied? |
|-----------|---------------|----------------|---------------|----------------------|
| 1         | 1000          | 1200           | -200          | No (Rollback)        |

---

## 2.3. Isolation

### What
- Isolation ensures that concurrent transactions do not interfere with each other. Each transaction is executed as if it were the only one in the system.

### Why
- Prevents issues like dirty reads, non-repeatable reads, and phantom reads.

### How

```sql
-- Transaction 1
BEGIN TRANSACTION;
UPDATE Accounts SET Balance = Balance - 100 WHERE AccountID = 1;

-- Transaction 2 (runs concurrently)
BEGIN TRANSACTION;
SELECT Balance FROM Accounts WHERE AccountID = 1; -- Should not see uncommitted changes from Transaction 1
```

| Time         | Transaction 1 (T1)         | Transaction 2 (T2)         | Observed Balance |
|--------------|----------------------------|----------------------------|------------------|
| Start        | Reads 1000                 | Reads 1000                 | 1000             |
| T1 Updates   | Sets to 900 (uncommitted)  | Reads 1000 (should not see T1's change) | 1000             |
| T1 Commits   | 900                        |                            |                  |

---

## 2.4. Durability

### What
- Durability ensures that once a transaction is committed, its changes are permanent, even in the event of a system failure.

### Why
- Guarantees that committed data will not be lost, providing reliability.

### How

```sql
BEGIN TRANSACTION;
UPDATE Accounts SET Balance = Balance + 200 WHERE AccountID = 2;
COMMIT TRANSACTION;
-- Even if the system crashes now, the change will persist after recovery.
```

| Event                | Account 2 Balance |
|----------------------|------------------|
| Before Transaction   | 600              |
| After Update         | 800              |
| After Commit         | 800              |
| After Crash/Restart  | 800              |


