# 📘 PL/SQL Chapter 6: Records

---

## 📌 What Are Records?

In PL/SQL, a **record** is a **composite data type** that can hold **multiple values** of **different types** — essentially, one **row** of a table.

### 🔎 Why Use Records?

Consider a row with 10+ columns. Without records, you need 10+ individual variables to store each column value. This:

- Makes your code **lengthy**
- Adds **maintenance overhead**
- Is **error-prone** as tables evolve

Using records:

- Simplifies code
- Allows handling of **entire rows** with a single variable
- Automatically adjusts to changes in table structure

---

## 🧪 Traditional Way: Using Multiple Variables

```plsql
PROCEDURE GET_CUSTOMER_DETAILS IS
  v_id CUSTOMER.CUSTOMER_ID%TYPE;
  v_fname CUSTOMER.FIRST_NAME%TYPE;
  v_lname CUSTOMER.LAST_NAME%TYPE;
  v_email CUSTOMER.EMAIL%TYPE;
  v_contact CUSTOMER.CONTACT%TYPE;
  -- And more for each column...
BEGIN
  SELECT CUSTOMER_ID, FIRST_NAME, LAST_NAME, EMAIL, CONTACT
  INTO v_id, v_fname, v_lname, v_email, v_contact
  FROM CUSTOMER
  WHERE CUSTOMER_ID = 1;

  DBMS_OUTPUT.PUT_LINE('Name: ' || v_fname || ' ' || v_lname);
END;
```

📉 **Disadvantage**: Not scalable for tables with many columns.

---

## ✅ Using Records: Cleaner & Easier

### 📜 Code Example

```plsql
PROCEDURE GET_CUSTOMER_DETAILS IS
  c_rec CUSTOMER%ROWTYPE;  -- Declaring a record variable
BEGIN
  SELECT *
  INTO c_rec
  FROM CUSTOMER
  WHERE CUSTOMER_ID = 1;

  DBMS_OUTPUT.PUT_LINE('Name: ' || c_rec.FIRST_NAME || ' ' || c_rec.LAST_NAME);
  DBMS_OUTPUT.PUT_LINE('Email: ' || c_rec.EMAIL);
  DBMS_OUTPUT.PUT_LINE('Contact: ' || c_rec.CONTACT);
END;
```

### 🌟 Benefits

- Only one record variable handles **all columns**
- Table structure changes? No need to modify your code
- Simpler and neater

📌 **Syntax**: `record_variable table_name%ROWTYPE`

---

## 🧪 Modifying and Copying Records

### ✏️ Updating Fields in a Record

```plsql
c_rec.FIRST_NAME := 'Sonu';
c_rec.LAST_NAME := 'SSSSS';
```

### 📄 Copying One Record to Another

```plsql
c_rec1 CUSTOMER%ROWTYPE;
c_rec1 := c_rec;
```

After copying, all fields from `c_rec` are transferred to `c_rec1`.

### 🔨 Display Output

```plsql
DBMS_OUTPUT.PUT_LINE('First Name: ' || c_rec.FIRST_NAME);
DBMS_OUTPUT.PUT_LINE('Last Name: ' || c_rec.LAST_NAME);
DBMS_OUTPUT.PUT_LINE('Copied First Name: ' || c_rec1.FIRST_NAME);
DBMS_OUTPUT.PUT_LINE('Copied Last Name: ' || c_rec1.LAST_NAME);
```

---

## 💡 Passing Records Between Procedures

You can pass a record as a parameter to another procedure.

### ✍️ Example

```plsql
-- Procedure A: Fetches the record
PROCEDURE PROCESS_CUSTOMER IS
  c_rec CUSTOMER%ROWTYPE;
BEGIN
  SELECT * INTO c_rec FROM CUSTOMER WHERE CUSTOMER_ID = 1;
  SHOW_CUSTOMER(c_rec);  -- Passing record to another procedure
END;

-- Procedure B: Accepts the record
PROCEDURE SHOW_CUSTOMER(customer_in IN CUSTOMER%ROWTYPE) IS
BEGIN
  DBMS_OUTPUT.PUT_LINE('First Name: ' || customer_in.FIRST_NAME);
  DBMS_OUTPUT.PUT_LINE('Last Name: ' || customer_in.LAST_NAME);
END;
```

### ✅ Notes

- Use `%ROWTYPE` in both procedures
- Allows clean modular design

---

## 🚀 Inserting with Record Data Type

You can also insert values using a record.

### 🕘️ Example

```plsql
PROCEDURE SHOW_CUSTOMER(customer_in IN CUSTOMER%ROWTYPE) IS
BEGIN
  INSERT INTO CUSTOMER VALUES customer_in;
  COMMIT;
END;
```

### 🗓 Scenario

- `PROCESS_CUSTOMER` fetches row from `CUSTOMER` table and stores in `c_rec`
- Passes `c_rec` to `SHOW_CUSTOMER`
- `SHOW_CUSTOMER` inserts `c_rec` values back to the table as a new row

---

## 🚀 Updating with Record Data Type

You can also update existing records using a record.

### 📅 Example

```plsql
PROCEDURE SHOW_CUSTOMER(customer_in IN CUSTOMER%ROWTYPE) IS
BEGIN
  UPDATE CUSTOMER
  SET ROW = customer_in
  WHERE CUSTOMER_ID = customer_in.CUSTOMER_ID;
  COMMIT;
END;
```

### 🔹 Notes

- First modify any field like: `c_rec.FIRST_NAME := 'Sonu';`
- Then pass it to a procedure to perform the update

---

## 🦾 User-Defined Record Types

You can define your own custom record types if your requirements don't match any table directly.

### 🛠 Syntax

```plsql
DECLARE
  TYPE customer_rec_type IS RECORD (
    fname VARCHAR2(50),
    lname VARCHAR2(50),
    email VARCHAR2(100)
  );

  c_rec customer_rec_type;
BEGIN
  c_rec.fname := 'Sonu';
  c_rec.lname := 'Singh';
  c_rec.email := 'sonu@example.com';
  DBMS_OUTPUT.PUT_LINE('Name: ' || c_rec.fname || ' ' || c_rec.lname);
END;
```

### ✅ When to Use

- When columns are from different tables or don't exist in any table
- When logic-specific structure is needed in code

### 💡 Full Example with SELECT

```plsql
PROCEDURE PROCESS_CUSTOMER IS
  TYPE customer_rec IS RECORD (
    FIRST_NAME VARCHAR2(50),
    LAST_NAME VARCHAR2(50)
  );
  c_rec customer_rec;
BEGIN
  SELECT FIRST_NAME, LAST_NAME
  INTO c_rec
  FROM CUSTOMER
  WHERE CUSTOMER_ID = 1;

  DBMS_OUTPUT.PUT_LINE('Name: ' || c_rec.FIRST_NAME || ' ' || c_rec.LAST_NAME);
END;
```

---

## ✨ Best Practices

- Use records when working with complete rows
- Prefer `%ROWTYPE` for better maintainability
- Access fields using: `record_name.column_name`
- To assign values: `record_name.column_name := value;`
- To copy records: `record2 := record1;`
- To pass records: declare formal parameter with `%ROWTYPE`
- To insert: `INSERT INTO table_name VALUES record_variable;`
- To update: `UPDATE table_name SET ROW = record_variable WHERE condition;`
- Use `TYPE ... IS RECORD` for user-defined records

---

## 🚀 Summary

Using **records** in PL/SQL makes your code **cleaner, scalable**, and **easier to maintain** — especially when dealing with large tables. You can also **update**, **copy**, **pass**, **insert**, and **define custom records**, simplifying your data handling operations.

