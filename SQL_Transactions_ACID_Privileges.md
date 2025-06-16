# 📌 SQL Essentials: Transactions, ACID, and User Privileges

---

## 🔁 Transactions

### 🧠 Why Are Transactions Important?

- Ensure operations **fully complete or fully fail**.
- Useful in cases like **bank transfers** – partial updates could lead to major data inconsistency.

### 💡 Concept

- A **transaction** groups multiple SQL operations to execute as a unit.
- It has states: start, commit (save), or rollback (undo).

### 🛠 Basic Syntax

```sql
-- Start a transaction
START TRANSACTION;

-- Execute SQL statements
UPDATE products SET price = 500 WHERE id = 5;
UPDATE products SET price = 500 WHERE id = 6;

-- Commit to make changes permanent
COMMIT;
```

### 🔄 Rollback (Undo on Error)

```sql
START TRANSACTION;

UPDATE products SET price = 500 WHERE id = 5;
-- An error occurs here
ROLLBACK; -- Reverts all changes since START
```

### ✅ Conditional Commit

```sql
START TRANSACTION;

SELECT * FROM products WHERE id = 5;
UPDATE products SET price = 500 WHERE id = 5;

IF (@correct) THEN
  COMMIT;
ELSE
  ROLLBACK;
END IF;
```

---

## 🧱 ACID Properties

### 🧪 Atomicity
- All or nothing.
- Example: Transfer A → B. If B doesn't get the money, A shouldn't lose it.

### 🔄 Consistency
- Keeps database constraints satisfied.
- Example: If an operation violates a constraint, rollback to maintain integrity.

### 🚪 Isolation
- Transactions should not interfere.
- Example: Two people withdrawing from the same account – locking helps maintain correctness.

### 🧍‍♂️ Durability
- Committed data survives crashes.
- Example: Power outage after COMMIT – data should still exist.

---

## 👥 User Privileges in SQL

### 👤 Creating a User

```sql
CREATE USER 'john'@'localhost' IDENTIFIED BY 'password';
```

- `'john'` = username
- `'localhost'` = network context (IP/%)
- `'password'` = account password

### 🌐 Network Context

| Context | Description |
|---------|-------------|
| `localhost` | User can connect only from local machine |
| `110.78.9.12` | Connect only from this IP |
| `%` | Connect from anywhere (⚠️ Risky!) |

### 🔍 Viewing Users

```sql
SELECT * FROM mysql.user;
```

### 🔑 Granting Permissions

```sql
GRANT ALL ON new_schema.orders TO 'john'@'localhost';
GRANT SELECT ON new_schema.* TO 'root'@'150.10.12.1';
```

- `GRANT [privilege] ON [db].[table] TO [user]@[host];`
- Common privileges: `ALL`, `SELECT`, `INSERT`, `UPDATE`, etc.

---

This file summarizes essential transaction mechanisms and permission management practices in SQL. It's crucial for ensuring **data reliability**, **security**, and **performance** in real-world database systems.
