# PL/SQL Chapter 8: Collections

## 📚 Collections Overview

### 📖 What Is a Collection?

A collection is a single-dimensional array that stores elements of the same data type. Every element has an index (starting typically at 1).

### 📌 Properties of Collections

- Elements are of the same data type
- Each element is associated with a unique index
- Can be used to hold multiple values in memory

### ⚖️ Difference Between RECORD and COLLECTION

| Feature            | RECORD                          | COLLECTION                         |
| ------------------ | ------------------------------- | ---------------------------------- |
| Structure Type     | Composite (group of fields)     | Array (single data type elements)  |
| Data Types Allowed | Can store multiple data types   | Stores only one data type          |
| Access By          | Field name (e.g. rec.name)      | Index (e.g. arr(1))                |
| Usage              | Represent a row of related data | Handle multiple values efficiently |
| Memory             | Fixed structure                 | Dynamically extendable             |
| Nested Use         | Cannot be nested easily         | Can be nested and used in loops    |

### 🧠 Why Use Collections?

Collections enable PL/SQL programs to benefit from performance optimization features like:

- **Bulk Collect**: Retrieve multiple rows from the database in a single fetch
- **Forall**: Perform DML (INSERT/UPDATE/DELETE) operations on collections efficiently
- **Table Functions**: Return a collection as a result set usable in SQL queries

These features help improve execution speed and reduce context switches between SQL and PL/SQL engines.

---

## 🧾 Collection Terminology

- **Index Value**: Location of an element in the collection (e.g. 1, 2, 3...)
- **Element**: The actual data (e.g. 'John', 'Kiran')
- **Dense Collection**: All indexes are filled (no gaps)
- **Sparse Collection**: Gaps exist in the index values
- **Method Examples**: `.FIRST`, `.NEXT`, `.LAST` to traverse and access elements

---

## 📘 Types of Collections in PL/SQL

There are three types:

1. **Index-by Tables (Associative Arrays)**
2. **Nested Tables**
3. **VARRAYs**

---

## 📌 Index-by Tables (Associative Arrays)

- Indexed using `BINARY_INTEGER` or `VARCHAR2`
- Cannot be stored in the database
- No need to initialize
- Memory is allocated dynamically
- Gaps are allowed (sparse)

### Syntax:

```plsql
TYPE customer_type IS TABLE OF VARCHAR2(100) INDEX BY BINARY_INTEGER;
customer_table customer_type;
customer_table(1) := 'Mike';
customer_table(6) := 'King';
```

### Traversal:

```plsql
v_idx := customer_table.FIRST;
WHILE v_idx IS NOT NULL LOOP
  DBMS_OUTPUT.PUT_LINE(customer_table(v_idx));
  v_idx := customer_table.NEXT(v_idx);
END LOOP;
```

---

## 📘 Nested Tables

- Can be stored in the database
- Must be initialized
- Memory must be extended using `.EXTEND`
- Indexed only by integer
- Supports multiset operations

### Syntax:

```plsql
TYPE customer_type IS TABLE OF VARCHAR2(100);
customer_table customer_type := customer_type();
customer_table.EXTEND(4);
customer_table(1) := 'Mike';
```

### Notes:

- Insertion must be sequential (1, 2, 3, ...)
- Deletion is allowed using `.DELETE(index)`

---

## 📗 VARRAYs

- Can be stored in the database
- Must specify the maximum size
- Must be initialized
- Memory must be extended using `.EXTEND`
- Indexed only by integer
- **No deletion allowed** (always dense)

### Syntax:

```plsql
TYPE customer_type IS VARRAY(4) OF VARCHAR2(100);
customer_table customer_type := customer_type();
customer_table.EXTEND(4);
customer_table(1) := 'Mike';
```

### Notes:

- Fixed number of elements
- All values must be inserted sequentially
- Attempting `.DELETE` throws an error

---

## 🧠 Summary

- Use **explicit cursors** for fine-grained control
- Prefer **cursor FOR loops** for concise syntax
- Combine **cursors with records** to manage multiple columns efficiently
- Use **SYS\_REFCURSOR** to return query results across program units
- Handle `CURSOR_ALREADY_OPEN` and `INVALID_CURSOR` exceptions to avoid runtime errors
- Use **collections** to optimize bulk operations with **Bulk Collect** and **Forall**
- Choose the right collection type based on storage, flexibility, and memory needs

### 🔹 Choosing the Right Collection Type

- If `TYPE ... INDEX BY` is used → **Associative Array**
- If `TYPE ... VARRAY(...)` is used → **VARRAY**
- If neither is used → **Nested Table**

### 🔹 Quick Guide

| Collection Type   | Indexed By   | Can Be Stored | Needs Initialization | Supports DELETE | Allows Gaps | Max Size Needed |
| ----------------- | ------------ | ------------- | -------------------- | --------------- | ----------- | --------------- |
| Associative Array | Integer/Text | ❌ No          | ❌ No                 | ✅ Yes           | ✅ Yes       | ❌ No            |
| Nested Table      | Integer Only | ✅ Yes         | ✅ Yes                | ✅ Yes           | ✅ Yes       | ❌ No            |
| VARRAY            | Integer Only | ✅ Yes         | ✅ Yes                | ❌ No            | ❌ No        | ✅ Yes           |

### 🔹 Recommendation

- Use **Associative Arrays** for most in-memory PL/SQL operations
- Use **Nested Tables** when storing collections in DB and using set operations
- Use **VARRAYs** only when number of elements is known and fixed

