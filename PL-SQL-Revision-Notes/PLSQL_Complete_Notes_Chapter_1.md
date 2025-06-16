# 📘 PL/SQL Notes

This document contains structured notes on key PL/SQL concepts, including block structure, variable declaration, control flow, and loops.

---

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

## 🧱 PL/SQL Block Structure

A typical PL/SQL block has **three** main sections:

```plsql
DECLARE
  -- Declarations: variables, cursors, types
BEGIN
  -- Executable statements: SQL operations, control logic
EXCEPTION
  -- Error handling: catch and handle exceptions
END;
/
```

---

## 🧾 Declaring Variables in PL/SQL

### ✍️ Syntax

```plsql
variable_name datatype [ := value | DEFAULT value ];
```

- `:=` – Assigns a default value using the assignment operator.
- `DEFAULT` – Alternate way to assign a default value.
- End each statement with a semicolon (`;`)

---

### ✅ Examples

```plsql
order_number NUMBER := 1001;
order_id NUMBER DEFAULT 1002;
customer_name VARCHAR2(20) := 'John';
```

---

## 🔐 CONSTANT Variables

```plsql
order_number CONSTANT NUMBER := 1001;
-- Reassignment in BEGIN will raise error
```

---

## 🌐 Global vs Local Variables in PL/SQL

### 🧱 Example: Nested Blocks

```plsql
DECLARE
  num1 NUMBER := 95;  -- Global
BEGIN
  DECLARE
    num2 NUMBER := 185;  -- Local
  BEGIN
    DBMS_OUTPUT.PUT_LINE('Inner Variable num1: ' || num1);
    DBMS_OUTPUT.PUT_LINE('Inner Variable num2: ' || num2);
  END;
  DBMS_OUTPUT.PUT_LINE('Outer Variable num1: ' || num1);
  -- num2 is not accessible here
END;
```

---

## 🔀 IF-THEN-ELSE in PL/SQL

```plsql
IF total_amount > 200 THEN
  discount := total_amount * 0.20;
ELSIF total_amount >= 100 AND total_amount <= 200 THEN
  discount := total_amount * 0.10;
ELSE
  discount := total_amount * 0.05;
END IF;
```

---

## 🎯 CASE Statement

```plsql
CASE 
  WHEN total_amount > 200 THEN
    discount := total_amount * 0.20;
  WHEN total_amount >= 100 AND total_amount <= 200 THEN
    discount := total_amount * 0.10;
  ELSE
    discount := total_amount * 0.05;
END CASE;
```

---

## 🔄 WHILE Loop

```plsql
counter NUMBER := 10;
WHILE counter < 20 LOOP
  DBMS_OUTPUT.PUT_LINE('Counter: ' || counter);
  counter := counter + 1;
END LOOP;
```

---

## 🔁 FOR Loop

```plsql
-- Forward
FOR i IN 10..20 LOOP
  DBMS_OUTPUT.PUT_LINE(i);
END LOOP;

-- Reverse
FOR i IN REVERSE 10..20 LOOP
  DBMS_OUTPUT.PUT_LINE(i);
END LOOP;
```

---

## 📌 Summary

| Construct   | Purpose                                  |
|-------------|-------------------------------------------|
| DECLARE     | Define variables, cursors, subprograms    |
| BEGIN       | Main logic and SQL execution              |
| EXCEPTION   | Catch and handle errors                   |
| IF/CASE     | Conditional logic                         |
| WHILE/FOR   | Loops for iterative operations            |

> Continue exploring procedures, functions, cursors, and triggers in PL/SQL for complete mastery!
