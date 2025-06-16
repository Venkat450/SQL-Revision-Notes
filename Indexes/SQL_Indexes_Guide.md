# 📌 SQL Indexes – Overview & Guide

## 📖 What is an Index?

An **index** is a data structure that improves the speed of data retrieval operations on a database table at the cost of additional storage and slower write operations.

**Analogy**:  
- Like a **book index**: helps locate pages quickly without flipping each one.  
- Like a **hotel room map**: allows faster navigation to the correct floor and room.

---

## 📂 Types of Indexes

### ✅ By Structure

1. **Clustered Index**
   - Physically sorts the table rows.
   - Only one allowed per table.
   - Data pages (leaves of the B-tree) contain **actual table data**.
   - Fast reads, slower inserts/updates.
   - Ideal for: primary keys, range queries, columns with rarely updated values.

2. **Non-Clustered Index**
   - Stores **pointers** (row locators) to actual data.
   - Data remains unordered (heap).
   - Allows multiple per table.
   - Leaf nodes contain index pages with pointers (not data).
   - Ideal for: frequently searched columns, joins, exact value lookups.

### ✅ By Storage

- **Row Store Index**: Default, stores data in rows.
- **Column Store Index**: Stores data column-wise, useful for analytics and aggregation-heavy queries.

### ✅ By Function

- **Unique Index**: Ensures all values in indexed column(s) are unique.
- **Filtered Index**: Index on a subset of rows (with a WHERE clause).

---

## 🏗 SQL Page Architecture

- **Page**: Unit of storage = 8 KB.
- **Sections**:
  - Header (metadata)
  - Data rows
  - Offset array (quick navigation)

---

## 📊 Heap Structure (No Index)

- Data inserted in arbitrary order.
- Requires **full table scans** for lookups.
- Fastest writes, slowest reads.

---

## 🌳 B-Tree Structure

- Used by both clustered and non-clustered indexes.
- Contains:
  - Root node
  - Intermediate nodes
  - Leaf nodes:
    - Clustered: data pages
    - Non-clustered: index pages with row locators

---

## 📌 Syntax Examples

```sql
-- Clustered Index
CREATE CLUSTERED INDEX idx_tb_customers_customerid
ON Sales.tb_customers (CustomerID);

-- Non-Clustered Index
CREATE NONCLUSTERED INDEX idx_tb_customers_lastname
ON Sales.tb_customers (LastName);

-- Composite Index
CREATE INDEX idx_tb_customers_country_score
ON Sales.tb_customers (Country ASC, Score DESC);
```

📝 *Note: If `CLUSTERED` or `NONCLUSTERED` is not specified, SQL Server creates a NONCLUSTERED index by default.*

---

## 📏 Composite Index & Leftmost Prefix Rule

- Indexes with multiple columns follow **leftmost prefix rule**:
  - `INDEX(A, B, C, D)`
  - Queries using:
    - `A`, `A, B`, `A, B, C`, `A, B, C, D` → ✅ uses index.
    - `B`, `C`, `B, C`, `C, D`, etc. → ❌ does not use index.

---

## ⚖️ Clustered vs Non-Clustered

| Feature | Clustered | Non-Clustered |
|--------|-----------|----------------|
| Data Sorted? | Yes | No |
| Leaf Nodes Contain | Actual data | Pointers (Row locators) |
| Max per Table | 1 | Many |
| Read Speed | Faster | Slightly slower |
| Write Speed | Slower | Faster |
| Storage | Efficient | More storage due to pointers |
| Best For | Primary key, range queries | Filters, joins, exact matches |

---

## 🛠 Maintenance

- Only one **clustered index** allowed.
- If you want to change it:
  ```sql
  DROP INDEX idx_tb_customers_customerid ON Sales.tb_customers;
  ```
