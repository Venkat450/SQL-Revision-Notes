
# 📘 SQL Revision Notes - Chapter 2 (Part 2): WHERE Clause and Conditions

## 📌 What You Will Learn

- Using WHERE clause for conditional filtering
- Applying single and multiple conditions
- Handling NULL values in conditions
- Using IN, BETWEEN, LIKE for range and fuzzy matching

---

## 📦 Basic WHERE Conditions

### Equal, Greater, Less, Not Equal

```sql
SELECT * FROM `new_schema`.`users` WHERE id = 1;
SELECT * FROM `new_schema`.`users` WHERE id > 2;
SELECT * FROM `new_schema`.`users` WHERE id <= 1;
SELECT * FROM `new_schema`.`users` WHERE id != 1;
```

- `=` → Equal to
- `!=` → Not equal to
- `>` → Greater than
- `<` → Less than
- `>=` → Greater than or equal to
- `<=` → Less than or equal to

---

### NULL Conditions

```sql
SELECT * FROM `new_schema`.`users` WHERE height IS NULL;
SELECT * FROM `new_schema`.`users` WHERE height IS NOT NULL;
```

- Use `IS NULL` and `IS NOT NULL` → `=` cannot be used with NULL

---

## 🔗 Multiple Conditions

### AND (All conditions must be True)

```sql
SELECT * FROM `new_schema`.`users` WHERE age < 40 AND height > 160;
```

### OR (Any condition True is enough)

```sql
SELECT * FROM `new_schema`.`users` WHERE age < 40 OR height > 160;
```

### AND + OR (Using Parentheses)

```sql
SELECT * FROM `new_schema`.`users` 
WHERE id < 4 AND (age > 30 OR height > 175);
```

✅ Parentheses are important to control precedence

---

## 🔎 Range and Set Conditions

### IN (Set of values)

```sql
SELECT * FROM `new_schema`.`users` WHERE id IN (1, 3);
SELECT * FROM `new_schema`.`users` WHERE id NOT IN (1, 4);
```

- `IN` → Match any of the listed values
- `NOT IN` → Exclude the listed values

### BETWEEN (Range)

```sql
SELECT * FROM `new_schema`.`users` WHERE height BETWEEN 160 AND 190;
```

- Fetches values between 160 and 190 inclusive

### LIKE (Fuzzy/Pattern Matching)

```sql
SELECT * FROM `new_schema`.`users` WHERE name LIKE '%a%';
SELECT * FROM `new_schema`.`users` WHERE name LIKE 'J%';
SELECT * FROM `new_schema`.`users` WHERE name LIKE '%y';
```

- `%` → Matches zero, one or many characters
- `_` → Matches exactly one character (not shown above but useful)

---

## ✅ Summary

| Condition Type | SQL Syntax | Notes |
|----------------|------------|-------|
| Exact match | = , != , > , < , >= , <= | Standard conditions |
| NULL check | IS NULL / IS NOT NULL | For missing values |
| Multiple conditions | AND / OR | Combine filters |
| Range | IN / NOT IN / BETWEEN | Set and range filters |
| Pattern search | LIKE | Fuzzy matching |

---

## 📎 Best Practices

- Always use parentheses when mixing AND / OR
- Prefer `IN` for multiple discrete values over `OR`
- Use `LIKE` carefully → avoid overuse in large datasets
- Always check NULL using `IS NULL` / `IS NOT NULL`

---
