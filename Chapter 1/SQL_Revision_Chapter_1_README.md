
# 📘 SQL Revision Notes - Chapter 1: Database Basics

## 🧱 SQL Database Architecture

A typical database structure follows this hierarchy:

```
Database Software (MySQL, PostgreSQL, etc)
    └── Database (Logical separation e.g. US Stock, London Stock)
        └── Schema (Group of Tables + Metadata)
            └── Table (Stores Rows and Columns)
                └── Data (Actual data)
```

✅ In MySQL, **Database** and **Schema** are the same.  
✅ Schema groups logical tables and controls structure + rules.

---

## 📦 Schema

A **Schema** is a collection of Tables + Metadata.  
You can create a new schema using:

```sql
CREATE SCHEMA `new_schema` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

**Notes:**

- `CREATE SCHEMA`: Create new schema
- `DEFAULT CHARACTER SET utf8mb4`: Defines character encoding
- `COLLATE utf8mb4_unicode_ci`: Defines collation rules (eg: emojis support)
- `;`: Always end SQL statements with semicolon

You can check existing schemas:

```sql
SELECT * FROM INFORMATION_SCHEMA.SCHEMATA;
```

---

## 📑 Table

A **Table** stores data in rows and columns.  
It manages:

1. **Metadata** (column data types, default values)
2. **Indexes** (for faster search)
3. **Relationships** (foreign keys)
4. **Physical Storage Location**

### Create Table Syntax

```sql
CREATE TABLE `new_schema`.`new_table` (
    `id` INT NOT NULL COMMENT 'Primary Key',
    PRIMARY KEY (`id`)
);
```

**Primary Key**:
- Uniquely identifies each row
- Cannot be NULL or duplicated

### Read Table

```sql
SHOW FULL COLUMNS FROM `new_schema`.`new_table`;
```

### Delete Table (Dangerous!)

```sql
DROP TABLE `new_schema`.`new_table`;
```

### Clean Table (Remove all data, keep table)

```sql
TRUNCATE `new_schema`.`new_table`;
```

---

## 📍 Column

Columns define **data types** and **attributes**:

### Data Types

- **Number** → BIGINT, INT, FLOAT, DECIMAL
- **Datetime** → DATE, DATETIME, TIMESTAMP
- **Text** → CHAR, VARCHAR, TEXT
- **Special** → BOOLEAN, JSON, BLOB

### Column Attributes

- `NOT NULL`: No NULL values allowed
- `AUTO_INCREMENT`: Automatically generate unique values
- `DEFAULT`: Provide a default value if no value is given

Example:

```sql
CREATE TABLE `new_schema`.`users` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(50) NOT NULL DEFAULT 'No Name',
    PRIMARY KEY (`id`)
);
```

### Update Columns

```sql
ALTER TABLE `new_schema`.`users`
ADD COLUMN `age` INT NULL AFTER `name`;

ALTER TABLE `new_schema`.`users`
CHANGE COLUMN `name` `user_name` VARCHAR(45) NOT NULL DEFAULT 'No Name';
```

**Notes**:
- `ALTER TABLE` → To change structure
- `ADD COLUMN` → Add new column
- `CHANGE COLUMN` → Rename + modify existing column
- `AFTER` → Defines the order of columns

---

# ✅ Summary

- **Database** → Organizes schemas
- **Schema** → Groups tables
- **Table** → Holds structured data
- **Column** → Defines the type of data and rules

These are the foundation of SQL. You need to master schema, table, and column definitions before writing complex queries!

---

# 📎 Reference

- Leetcode SQL Book (Basics)
