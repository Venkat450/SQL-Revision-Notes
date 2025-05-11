
# 📘 SQL Revision Notes - Chapter 2 (Part 4): Auxiliary SELECT Statements

## 📌 What You Will Learn

- How to avoid duplicate results using DISTINCT
- How to paginate results using LIMIT and OFFSET
- How to sort results using ORDER BY
- How to group data using GROUP BY

---

## 📦 Sample Table

| id | name | age | height |
|----|------|-----|--------|
| 1  | John | 40  | 150    |
| 2  | May  | 30  | 140    |
| 3  | Tim  | 25  | 170    |
| 4  | Jay  | 60  | 185    |
| 5  | Maria| 30  | 190    |
| 6  | Tom  | 53  | 200    |
| 7  | Carter| 40 | 145    |

---

## 🚫 Avoid Duplicates: DISTINCT

```sql
SELECT DISTINCT age 
FROM `new_schema`.`users`;
```

✅ Only unique ages are returned.

---

## 📄 Pagination: LIMIT and OFFSET

```sql
SELECT * 
FROM `new_schema`.`users`
LIMIT 3 OFFSET 1;
```

- `LIMIT`: How many rows to return
- `OFFSET`: How many rows to skip

✅ Used for pagination → displaying results page by page.

---

## 🔃 Sorting: ORDER BY

```sql
SELECT * 
FROM `new_schema`.`users`
ORDER BY age ASC;
```

- ASC → Ascending (default)
- DESC → Descending

### Multi-column sorting

```sql
SELECT * 
FROM `new_schema`.`users`
ORDER BY age DESC, height DESC;
```

✅ When `age` values are same → sorted by `height`.

---

## 🧹 Grouping: GROUP BY

```sql
SELECT `age` 
FROM `new_schema`.`users`
GROUP BY age;
```

- Similar to `DISTINCT` but powerful when combined with aggregates.

### Grouping + Counting

```sql
SELECT COUNT(*), `age`
FROM `new_schema`.`users`
GROUP BY age;
```

✅ Shows how many people fall into each `age` group.

### Grouping + Counting + Sorting + AS

```sql
SELECT COUNT(*) AS `age_count`, `age`
FROM `new_schema`.`users`
GROUP BY age
ORDER BY `age_count`;
```

✅ Cleaner output → sorted by count

---

## ✅ Summary

| Function | SQL Keyword | Usage |
|----------|-------------|-------|
| Remove duplicates | DISTINCT | Get unique values |
| Pagination | LIMIT + OFFSET | Return specific number of rows |
| Sorting | ORDER BY | Sort ascending/descending |
| Grouping | GROUP BY | Aggregate/group similar data |

---

## 📎 Best Practices

- Use `ORDER BY` at the end → improves readability
- Use `LIMIT` and `OFFSET` for paging large data
- `GROUP BY` is very powerful when combined with `COUNT()`, `SUM()`, `AVG()` etc
- Always use `AS` to make result column names user-friendly

---
