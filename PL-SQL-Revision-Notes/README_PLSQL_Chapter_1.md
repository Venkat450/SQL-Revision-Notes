# 📘 Introduction to PL/SQL

## 🧠 What is PL/SQL?

PL/SQL (Procedural Language/Structured Query Language) is Oracle's procedural extension for SQL. It allows the combination of SQL statements with procedural features like loops and conditions.

- ✅ SQL: Used to **fetch and manipulate** data.
- 🔁 Procedural Logic: Used to **control flow**, implement **business logic**, and process data efficiently.

> **Think of PL/SQL as adding programming power to SQL.**

---

## ❓ Why SQL Alone Isn't Enough

| Limitation in SQL | How PL/SQL Solves It |
|-------------------|----------------------|
| ❌ No loops or conditions | ✅ Supports control flow (IF, LOOP, CASE) |
| ❌ Executes one SQL statement at a time | ✅ Reduces network traffic with block execution |
| ❌ System-defined errors only | ✅ Customizable error handling with `EXCEPTION` block |
| ❌ No reusability | ✅ Code can be stored as **procedures/functions** |
| ❌ No modular programming | ✅ Supports **modularity, encapsulation,** and **object-oriented-like** features |

---

## 🔧 Structure of a PL/SQL Block

```sql
DECLARE
  -- Variable declarations
BEGIN
  -- Executable SQL and logic statements
EXCEPTION
  -- Error handling
END;
```

- **DECLARE**: Optional section to define variables and constants.
- **BEGIN**: Main block where SQL and procedural code are written.
- **EXCEPTION**: Optional block to handle run-time errors.

> 🧩 The block executes top to bottom, just like a mini-program inside the database.

---

## 📌 Summary

- PL/SQL blends SQL with structured programming.
- It supports loops, conditionals, modularity, error handling, and more.
- It reduces the limitations of plain SQL by supporting complex business logic directly in the database.

---

## 🔗 Next Topics

You might want to explore:
- Variables and data types in PL/SQL
- Control statements (`IF`, `LOOP`, `CASE`)
- Cursors and exceptions
- Stored procedures and functions
