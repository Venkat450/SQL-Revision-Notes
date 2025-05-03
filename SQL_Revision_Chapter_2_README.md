
# 📘 SQL Revision Notes - Chapter 2: Basic SQL Syntax (SELECT, INSERT, UPDATE, DELETE)

## 📌 What You Will Learn

- Basic syntax to **Read, Create, Modify, and Remove** data.
- How CRUD operations (Create, Read, Update, Delete) are performed in SQL.
- Best practices for simple and multiple record inserts.
- Usage of SELECT with filters (WHERE).
- How to modify (UPDATE) and remove (DELETE) data safely.

---

## 📦 SELECT (Read Data)

```sql
SELECT `id`, `name` 
FROM `new_schema`.`users`;
```

- `SELECT`: Specify which columns to fetch.
- `FROM`: Specify from which schema and table to fetch.

### Select All Columns

```sql
SELECT * 
FROM `new_schema`.`users`;
```

⚡️ *Note*: Avoid `SELECT *` in large tables → performance hit.

### Select With Condition (WHERE)

```sql
SELECT * 
FROM `new_schema`.`users` 
WHERE `id` = 2;
```

- Filters result to rows meeting the condition.

---

## ➕ INSERT (Create Data)

### Insert One Row

```sql
INSERT INTO `new_schema`.`users` (`id`, `name`, `age`) 
VALUES (4, 'Harry', 33);
```

- `INSERT INTO`: Target table and columns.
- `VALUES`: Insert values in the same order as columns.

### Insert Multiple Rows

```sql
INSERT INTO `new_schema`.`users` (`id`, `name`, `age`) 
VALUES (4, 'Harry', 33), (5, 'Tom', 30);
```

- Adds multiple records at once → efficient for bulk insertions.

---

## 🔄 UPDATE (Modify Data)

```sql
UPDATE `new_schema`.`users` 
SET `name` = 'Andy', `age` = 100 
WHERE `id` = 2;
```

- `UPDATE`: Specify table.
- `SET`: Assign new values.
- `WHERE`: Optional but important to avoid updating all rows accidentally.

✅ Always use `WHERE` to prevent full table updates.

---

## ❌ DELETE (Remove Data)

```sql
DELETE FROM `new_schema`.`users` 
WHERE `id` = 1;
```

- `DELETE FROM`: Specify the table.
- `WHERE`: Limit the rows to delete → avoid deleting everything.

⚡️ *Note*: Without `WHERE`, **entire table rows will be deleted**!

---

## ✅ Summary

| Operation | SQL Keyword | Notes |
|-----------|-------------|-------|
| Create    | INSERT      | Add new records |
| Read      | SELECT      | Fetch records |
| Update    | UPDATE      | Modify existing records |
| Delete    | DELETE      | Remove records |

---

## 📎 Best Practices

- Always use `WHERE` in `UPDATE` and `DELETE` queries.
- Avoid `SELECT *` unless necessary → better performance.
- Use `INSERT` with multiple rows when possible to save queries.
- Use descriptive column names and maintain readability.

---
