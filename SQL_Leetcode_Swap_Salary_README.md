
# 💻 SQL Leetcode Problem: Swap Salary

## Problem Description

**Table:** Salary

| Column Name | Type   |
|-------------|--------|
| id          | int    |
| name        | varchar |
| sex         | ENUM ('m', 'f') |
| salary      | int    |

- `id` is the primary key.
- `sex` can be 'm' or 'f'.
- The task is to swap the values of `sex` ('m' <-> 'f') with a **single UPDATE statement**, without using temporary tables.

## Example

**Input:**

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |

**Output:**

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |

## ✅ Solution (PostgreSQL)

```sql
UPDATE salary 
SET sex = CASE 
             WHEN sex = 'f' THEN 'm'
             ELSE 'f'
          END;
```

## 📌 Explanation

- `CASE` is used to conditionally check each row's `sex` value.
- If `sex` is `f`, it is changed to `m`.
- If `sex` is `m`, it is changed to `f`.
- No temporary tables or SELECT statements were used, fulfilling the problem requirement.

## 🚀 Best Practice Tip

- `CASE` expressions are very useful when updates need conditional logic without joins or subqueries.
- Use this pattern for simple conditional transformations in UPDATE statements.

---
