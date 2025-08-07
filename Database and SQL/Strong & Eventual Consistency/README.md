# Strong Consistency vs Eventual Consistency

In distributed systems, **consistency** refers to how up-to-date and synchronized copies of data are across different nodes. Two common models are **Strong Consistency** and **Eventual Consistency**.

---

## 1. Strong Consistency

### What
- Guarantees that after an update, any subsequent read will return the most recent value.
- All users see the same data at the same time, no matter which node they access.

### Example: Fintech (Banking Transaction)
Suppose you transfer $1000 from your savings to your checking account using a banking app.

**Scenario:**
1. You initiate a transfer.
2. The system deducts $1000 from savings and adds $1000 to checking.
3. If you immediately check your balances (from any device or branch), both reflect the updated values.

**SQL-like Example:**
```sql
-- Transfer $1000 from savings to checking
BEGIN TRANSACTION;
UPDATE Accounts SET Balance = Balance - 1000 WHERE AccountID = 'SAVINGS';
UPDATE Accounts SET Balance = Balance + 1000 WHERE AccountID = 'CHECKING';
COMMIT TRANSACTION;

-- Any read after commit will show the updated balances
SELECT Balance FROM Accounts WHERE AccountID IN ('SAVINGS', 'CHECKING');
```

**Table:**

| Time         | Savings Balance | Checking Balance | User Sees |
|--------------|-----------------|------------------|-----------|
| Before Txn   | $10,000         | $2,000           | $10,000 / $2,000 |
| After Txn    | $9,000          | $3,000           | $9,000 / $3,000  |

**Key Point:**  
No matter where or when you check, you always see the latest, correct balances.

---

## 2. Eventual Consistency

### What
- Guarantees that, given enough time without new updates, all nodes will eventually have the same data.
- Reads may return stale (outdated) data for a period after a write.

### Example: Social Media (Like Count)
Suppose you "like" a friend's photo on a social media platform.

**Scenario:**
1. You tap "like" on a photo.
2. Your action is recorded on one server and then propagated to others.
3. Your friend (or other users) may not immediately see the updated like count, especially if they are connected to a different server.
4. After a short delay, all servers synchronize, and everyone sees the correct like count.

**Pseudo-code Example:**
```python
# User A likes a photo
photo.likes += 1
# The update is sent to other servers asynchronously

# User B checks the like count immediately (may see old value)
print(photo.likes)  # Might be outdated

# After synchronization
print(photo.likes)  # Shows the correct, updated value
```

**Table:**

| Time                | Server 1 (User A) | Server 2 (User B) | User Sees |
|---------------------|-------------------|-------------------|-----------|
| Before Like         | 100               | 100               | 100       |
| After User A Likes  | 101               | 100               | 101 / 100 |
| After Sync          | 101               | 101               | 101       |

**Key Point:**  
Users may see different values temporarily, but eventually, all see the same, correct value.

---

## Summary Table

| Model               | Immediate Consistency | Temporary Stale Reads | Use Case Example      |
|---------------------|----------------------|----------------------|-----------------------|
| Strong Consistency  | Yes                  | No                   | Banking, Fintech      |
| Eventual Consistency| No                   | Yes                  | Social