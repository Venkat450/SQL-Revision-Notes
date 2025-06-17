# 📘 PL/SQL Chapter 7: Cursors

---

## 🌀 What Are Cursors?

When Oracle processes a SQL statement, it creates a **context area** in memory that stores all necessary information for processing that SQL. A **cursor** is a pointer to that context area.

There are two types of cursors:

- **Implicit Cursors** – Created automatically by Oracle for DML/SELECT INTO statements
- **Explicit Cursors** – Defined manually by the programmer for more control

---

## 📌 Implicit Cursors

### 📖 Definition

Oracle **automatically creates** an implicit cursor for **every DML** (INSERT, UPDATE, DELETE) or **SELECT INTO** statement.

This cursor is named `SQL` and provides four useful attributes:

| Attribute      | Description                                                            |
| -------------- | ---------------------------------------------------------------------- |
| `SQL%FOUND`    | TRUE if one or more rows are affected/fetched                          |
| `SQL%NOTFOUND` | TRUE if zero rows are affected/fetched                                 |
| `SQL%ROWCOUNT` | Number of rows affected/fetched                                        |
| `SQL%ISOPEN`   | Always FALSE for implicit cursors, as Oracle closes them automatically |

### ✅ When Are Implicit Cursors Used?

- INSERT → holds data to be inserted
- UPDATE/DELETE → identifies affected rows
- SELECT INTO → fetches single row

### 🧪 Example

```plsql
PROCEDURE PROCESS_CUSTOMER IS
  c_rec CUSTOMER%ROWTYPE;
  total_rows NUMBER;
BEGIN
  SELECT *
  INTO c_rec
  FROM CUSTOMER
  WHERE CUSTOMER_ID = 16;

  IF SQL%FOUND THEN
    total_rows := SQL%ROWCOUNT;
    DBMS_OUTPUT.PUT_LINE('Customer Selected.');
    DBMS_OUTPUT.PUT_LINE('First Name: ' || c_rec.FIRST_NAME);
    DBMS_OUTPUT.PUT_LINE('Last Name: ' || c_rec.LAST_NAME);
    DBMS_OUTPUT.PUT_LINE('Total Rows Fetched: ' || total_rows);
  END IF;
END;
```

### 🔍 Output Sample

```text
Customer Selected.
First Name: Sonu
Last Name: Afonso
Total Rows Fetched: 1
```

### 🔍 Summary

- Implicit cursors are **auto-managed** by Oracle.
- They simplify data access but offer **limited control**.
- Use SQL attributes to check execution status.

---

## ↔️ Explicit Cursors

### 📖 Definition

Explicit cursors are manually defined and controlled by the programmer for handling multiple row queries. They allow full control over:

- Declaration
- Opening
- Fetching
- Closing

### 🧪 Single Row Example

```plsql
PROCEDURE PROCESS_CUSTOMER IS
  c_rec CUSTOMER%ROWTYPE;
  CURSOR c IS
    SELECT * FROM CUSTOMER WHERE CUSTOMER_ID = 16;
BEGIN
  OPEN c;
  FETCH c INTO c_rec;
  DBMS_OUTPUT.PUT_LINE('First Name: ' || c_rec.FIRST_NAME);
  DBMS_OUTPUT.PUT_LINE('Last Name: ' || c_rec.LAST_NAME);
  CLOSE c;
END;
```

### 🧺 Multiple Rows Using LOOP

```plsql
PROCEDURE PROCESS_CUSTOMER IS
  c_rec CUSTOMER%ROWTYPE;
  CURSOR c IS
    SELECT * FROM CUSTOMER WHERE CUSTOMER_ID = 10;
BEGIN
  OPEN c;
  LOOP
    FETCH c INTO c_rec;
    EXIT WHEN c%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE('First Name: ' || c_rec.FIRST_NAME);
    DBMS_OUTPUT.PUT_LINE('Last Name: ' || c_rec.LAST_NAME);
  END LOOP;
  CLOSE c;
END;
```

### 🔄 Using Cursor with Record Data Type

```plsql
PROCEDURE PROCESS_CUSTOMER IS
  CURSOR c IS
    SELECT FIRST_NAME, LAST_NAME, MIDDLE_NAME FROM CUSTOMER WHERE CUSTOMER_ID = 10;
  c_rec c%ROWTYPE;
BEGIN
  OPEN c;
  LOOP
    FETCH c INTO c_rec;
    EXIT WHEN c%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE('First Name: ' || c_rec.FIRST_NAME);
    DBMS_OUTPUT.PUT_LINE('Middle Name: ' || c_rec.MIDDLE_NAME);
    DBMS_OUTPUT.PUT_LINE('Last Name: ' || c_rec.LAST_NAME);
  END LOOP;
  CLOSE c;
END;
```

---

## 🔁 Cursor FOR Loop

### 📖 Overview

Cursor FOR loops simplify the process of handling multiple rows by **automatically**:

- Declaring the cursor
- Opening the cursor
- Fetching rows one by one
- Closing the cursor

Oracle internally fetches **100 rows at a time**, even though it processes one row per iteration.

### 🧪 Example

```plsql
PROCEDURE PROCESS_CUSTOMER IS
BEGIN
  FOR c_rec IN (SELECT FIRST_NAME, LAST_NAME FROM CUSTOMER WHERE CUSTOMER_ID = 10) LOOP
    DBMS_OUTPUT.PUT_LINE('First Name: ' || c_rec.FIRST_NAME);
    DBMS_OUTPUT.PUT_LINE('Last Name: ' || c_rec.LAST_NAME);
  END LOOP;
END;
```

### ✅ Benefits

- No need to manually declare/open/fetch/close
- Code is more concise and readable
- Preferred for simple cursor operations

---

## 🔁 Cursor Variables and SYS\_REFCURSOR

### 📖 Definition

A cursor variable (also called a **REF CURSOR**) is a pointer to a query result set that can be **passed between procedures/functions**.

To create a cursor variable, use the predefined `SYS_REFCURSOR` type.

### 🧪 Example

```plsql
-- Function returning a SYS_REFCURSOR
CREATE OR REPLACE FUNCTION GET_NAMES(p_id NUMBER)
RETURN SYS_REFCURSOR IS
  c SYS_REFCURSOR;
BEGIN
  OPEN c FOR SELECT FIRST_NAME, LAST_NAME FROM CUSTOMER WHERE CUSTOMER_ID = p_id;
  RETURN c;
END;

-- Procedure that consumes the cursor
CREATE OR REPLACE PROCEDURE DISPLAY_NAMES IS
  c_rec SYS_REFCURSOR;
  fname VARCHAR2(50);
  lname VARCHAR2(50);
BEGIN
  c_rec := GET_NAMES(10);
  LOOP
    FETCH c_rec INTO fname, lname;
    EXIT WHEN c_rec%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE('First Name: ' || fname);
    DBMS_OUTPUT.PUT_LINE('Last Name: ' || lname);
  END LOOP;
  CLOSE c_rec;
END;
```

### ✅ Use Cases

- Returning query results to external clients
- Sharing result sets across PL/SQL blocks
- Building flexible and reusable procedures/functions

---

## 🚨 System Defined Exceptions with Cursors

### ❗ Common Exceptions

| Exception Name        | When It Occurs                                                |
| --------------------- | ------------------------------------------------------------- |
| `CURSOR_ALREADY_OPEN` | When you try to OPEN a cursor that is already open            |
| `INVALID_CURSOR`      | When you try to FETCH or CLOSE a cursor that was never opened |

### ✅ Best Practices

- Always check if the cursor is already open before opening.
- Never fetch or close a cursor unless it is open.
- Use exception handlers to catch and gracefully handle these errors.

```plsql
BEGIN
  OPEN my_cursor;
  OPEN my_cursor; -- Will raise CURSOR_ALREADY_OPEN
EXCEPTION
  WHEN CURSOR_ALREADY_OPEN THEN
    DBMS_OUTPUT.PUT_LINE('Cursor is already open.');
END;
```

---

### 🧠 Summary

- Use **explicit cursors** for fine-grained control.
- Prefer **cursor FOR loops** when you want brevity and automatic management.
- Combine **cursors with records** to manage multiple columns more efficiently.
- Use **SYS\_REFCURSOR** to return query results from functions or share them across program units.
- Handle **CURSOR\_ALREADY\_OPEN** and **INVALID\_CURSOR** exceptions to avoid runtime errors.

Next up: **Parameterized Cursors**!

