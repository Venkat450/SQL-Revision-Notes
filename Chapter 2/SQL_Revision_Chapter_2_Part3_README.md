
# 📘 SQL Revision Notes - Chapter 2 (Part 3): JSON in SQL

## 📌 What You Will Learn

- What is JSON and why it's important in modern databases
- How to store, read, and modify JSON formatted data in SQL
- How to query and filter JSON data

---

## 📦 What is JSON?

**JSON (JavaScript Object Notation)**

- Text-based format to store data
- Simple, human-readable and machine-friendly
- Key-value pairs wrapped in `{}` and arrays in `[]`

**Example JSON:**

```json
{
    "orderId": 54321,
    "info": [
        {"productID": 34, "productName": "productOne", "quantity": 1},
        {"productID": 56, "productName": "productTwo", "quantity": 3}
    ],
    "orderCompleted": true
}
```

---

## 📥 Storing JSON in SQL

```sql
ALTER TABLE `new_schema`.`users` 
ADD COLUMN `contact` JSON NULL AFTER `id`;
```

- JSON columns allow storage of JSON data
- Easier to query and manipulate later

---

## 📖 Reading JSON data

```sql
SELECT `id`, JSON_EXTRACT(contact, '$.phone') AS phone
FROM `new_schema`.`users`;
```

✅ `JSON_EXTRACT`: Pulls JSON values by key  
✅ `AS`: Renames the column in output

### Removing quotes:

```sql
SELECT `id`, JSON_UNQUOTE(JSON_EXTRACT(contact, '$.phone')) AS phone
FROM `new_schema`.`users`;
```

✅ `JSON_UNQUOTE`: Removes extra double quotes in output

---

## 🔍 Filtering JSON data (WHERE with JSON)

```sql
SELECT `id`, JSON_UNQUOTE(JSON_EXTRACT(contact, '$.phone')) AS phone
FROM `new_schema`.`users`
WHERE JSON_EXTRACT(contact, '$.phone') like '%456%';
```

✅ Use JSON_EXTRACT inside WHERE to filter rows based on JSON fields

---

## ➕ Adding JSON Data

```sql
INSERT INTO `new_schema`.`users` (`id`, `name`, `contact`) 
VALUES (5, 'Harry', JSON_OBJECT('phone', '1231123', 'address', 'Miami'));
```

✅ `JSON_OBJECT`: Constructs JSON format easily during insert

---

## 🔄 Updating JSON Data

```sql
UPDATE `new_schema`.`users` 
SET `contact` = JSON_SET(contact, '$.phone', '6666', '$.phone_2', '888') 
WHERE `id` = 2;
```

✅ `JSON_SET`: Modify existing key or add new key-value if not exists  
✅ Previous keys remain unchanged unless overwritten

---

## ✅ Summary

| Operation | SQL Function | Description |
|-----------|--------------|-------------|
| Extract JSON value | JSON_EXTRACT | Get specific field from JSON |
| Remove quotes | JSON_UNQUOTE | Remove extra quotes from result |
| Add new JSON | JSON_OBJECT | Insert new JSON |
| Update JSON | JSON_SET | Modify or add key-value pairs |

---

## 📎 Best Practices

- Use `JSON_UNQUOTE` to clean outputs for readability
- Use `JSON_SET` to update without losing existing JSON data
- JSON allows flexibility but comes with performance costs → avoid overusing for heavily relational data

---
